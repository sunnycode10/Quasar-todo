<template>
  <q-page class="bg-grey-3 column" > 
    <div class="row q-pa-sm bg-primary">
      <q-input filled bg-color="white" square class="col" v-model="newTask" placeholder="Add Task"  dense @keyup.enter="addTask" >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>
  <q-list class="bg-white" separator bordered> 
      <q-item  v-ripple v-for="(task, index) in tasks" :key="task.title" @click="task.done =!task.done" clickable :class="{'done':task.done}">
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primary" class="no-pointer-events" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side> 
    <q-btn flat round  dense color="primary" icon="delete" @click.stop="deleteTask(index)" />
        </q-item-section>
      </q-item>
    </q-list>
    <div class="no-task absolute-center" v-if="!tasks.length">
      <q-icon name="check" color="primary" size="100px"/>
      <div class="text-h5 text-primary text-center">
        No Task
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data(){
    return{
      newTask: '',
      tasks: [
        // {
        //   title: "Get bananas",
        //   done: false
        // },
        //  {
        //   title: "Eat bananas",
        //   done: true
        // },
        //   {
        //   title: "poo bananas",
        //   done: false
        // }
      ]
    }
  },
  methods:{
    deleteTask(index){
        this.$q.dialog({
          title: 'Confirm',
          message: 'do you wish to proceed ?',
          cancel: true,
          persistent: true
        }).onOk(() => {
            this.tasks.splice(index, 1)
            this.$q.notify('task deleted')
          })
    },
    addTask(){
        // console.log('add task')
      if (this.newTask.length != 0){
        this.tasks.push({
        title: this.newTask,
        done:false
      })
      this.newTask  = ""
      }else{
        this.$q.notify('task must not be empty')
      }
    }
  }
}
</script>
<style lang="scss"> 
.done{
    text-decoration: line-through;
    color: #bbb;
}
.no-task{
  opacity: 0.5;
}
</style>