<template>
   
    <AddTask v-show="showAddTask" @add-task="addTask"/>
    <Tasks :tasks="tasks" @delete-task="deleteTask" @toggle-reminder="toggleReminder"/>
    
</template>

<script>
import Tasks from '../components/Tasks';
import AddTask from '../components/AddTask';
export default {
    name:"Home",
    props:{
        showAddTask:Boolean,
    },
    components:{
        AddTask,
        Tasks
    },
    data(){
        return{
           tasks:[] 
        }
        
        
    },
    methods:{
        
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