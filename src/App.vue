<template>
  <div class="container">
    <Header title="Task Tracker" @toggle-add-task="toggleAddTask" :showAddTask="showAddTask" />
    <div v-if="showAddTask">
      <AddTask @add-task="addTask"/>
    </div>
    
    <Tasks :tasks="tasks" @delete-task="deleteTask" @toggle-reminder="toggleReminder"/>
  </div>
  
</template>

<script>

import Header from './components/Header';
import Tasks from './components/Tasks';
import AddTask from './components/AddTask';

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask,
  },
  data(){
    return {
      tasks:[],
      showAddTask:false,
    } 
  },
  methods:{
    toggleAddTask(){
       this.showAddTask = !this.showAddTask;
    },
    async addTask(task){
        const addNewTask = await fetch(`http://localhost:5000/tasks`,{
          method:"POST",
          headers:{
            "Content-Type":"application/json",
          },
          body:JSON.stringify(task)
        })
        const newTask = await addNewTask.json();
        console.log(newTask,task)
        this.tasks = [...this.tasks,{...newTask,...task}];
    },
    async deleteTask(id){
        const deleteTask = await fetch(`http://localhost:5000/tasks/${id}`,{
          method:"delete",             
        });
      await deleteTask.json();
      this.tasks = this.tasks.filter(task => task.id !== id)
        
    },
    async toggleReminder(id){
      let eTask = this.tasks.filter(task => task.id === id);
      eTask = eTask[0];
      eTask.reminder= !eTask.reminder;
      const updateTask = await fetch(`http://localhost:5000/tasks/${id}`,{
        method:"PUT",
        headers:{
          "Content-Type":"application/json",
        },
        body:JSON.stringify(eTask)
      })
      const updatedTask = await updateTask.json();
      this.tasks = this.tasks.map(task => (task.id === id? {...updatedTask}:task))
    },

    async fetchTask(){
      const res = await fetch('http://localhost:5000/tasks');
      const data = await res.json();
      //console.log(data);
      return data
    }
  },
  async created(){
    this.tasks = await this.fetchTask();
  }
}
</script>

<style>
/* You can add global styles to this file, and also import other style files */

@import url("https://fonts.googlehttp://localhost:5000s.com/css2?family=Poppins:wght@300;400&display=swap");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Poppins", sans-serif;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}

.form-control {
  margin: 20px 0;
}

.form-control label {
  display: block;
}

.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}

.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.form-control-check label {
  flex: 1;
}

.form-control-check input {
  flex: 2;
  height: 20px;
}
</style>
