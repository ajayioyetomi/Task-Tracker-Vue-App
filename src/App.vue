<template>
  <div class="container">
    <Header title="Task Tracker" />
    <Tasks :tasks="tasks" @delete-task="deleteTask" @toggle-reminder="toggleReminder"/>
  </div>
  
</template>

<script>

import Header from './components/Header';
import Tasks from './components/Tasks'
export default {
  name: 'App',
  components: {
    Header,
    Tasks
  },
  data(){
    return {
      tasks:[]
    } 
  },
  methods:{
    deleteTask(id){
        if(comfirm('Are you sure?')){
           this.tasks = this.tasks.filter(task => task.id !== id)
        } 
    },
    toggleReminder(id){
      this.tasks = this.tasks.map(task => (task.id === id? {...task,reminder:!task.reminder}:task))
    }
  },
  created(){
    this.tasks = [
        {
          id: 1,
          text: 'Doctors Appointment',
          day: 'May 5th at 2:30pm',
          reminder: true,
        },
        {
          id: 2,
          text: 'Meeting at School',
          day: 'May 6th at 1:30pm',
          reminder: true,
        },
        {
          id: 3,
          text: 'Food Shopping',
          day: 'May 7th at 12:30pm',
          reminder: false,
        }
    ]
  }
}
</script>

<style>
/* You can add global styles to this file, and also import other style files */

@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");

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
