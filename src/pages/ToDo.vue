<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input class="col" filled  v-model="newTask" placeholder="Add Task" dense  bg-color="white" square @keyup.enter="addTask">
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask"></q-btn>
        </template>
      </q-input>
    </div>
    <!-- <q-input v-model="todoList" style="width:80%" label="Input Todo" class="q-ml-lg" /> -->
<q-list class="bg-white" separator bordered >
      <q-item v-ripple v-for="(task, index ) in tasks" :key="task.title" @click="task.done = !task.done" :class="{ 'done bg-blue-1': task.done}" clickable>
        <q-item-section avatar>
          <q-checkbox v-model="task.done"  color="primary" class="no-pointer-events" />
        </q-item-section>
        <q-item-section v-model="task.done" side>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn @click.stop="deleteTask(index)" rounded dense color="primary" icon="delete"></q-btn>
        </q-item-section>
      </q-item>
    </q-list>
    <div class="no-task absolute-center" v-if="!tasks.length" >
      <q-icon name="check" size="100px" color="primary"></q-icon>
      <div class="text-h5 text-primary text-center">
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'

export default{
  data(){
  return{
    newTask:'',
    tasks:[
      {
        title:'Get bananas',
        done: false
      },{
        title:'Eat bananas',
        done: false
      },{
        title:'Poo bananas',
        done: false
      },
    ]
  }
  },
  methods:{
    deleteTask(index){
      this.$q.dialog({
        title: 'Confirm',
        message: 'Are you sure you really want to delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1)
        this.$q.notify('Task has been deleted')
      })
    },
    addTask(){
this.tasks.push({
  title : this.newTask,
  done: false
})
this.newTask = ''
    }
  }
}
</script>
<style lang="scss">
.done{
  .q-item__label{
    text-decoration: line-through;
    color: #bbb;
  }
}
</style>
