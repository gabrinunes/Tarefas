<template>
	<div id="app">
		<h1>Tarefas</h1>
		<TaskProgress :progress="progress"/>
		<NewTask @taskAdded="addTask"/>
		<TaskGrid @taskDeleted="deleteTask"
		@taskStateChanged="toggleTaskState"
		:tasks="task"/>
	</div>
</template>

<script>
import TaskProgress from './components/TaskProgress'
import NewTask from './components/NewTask'
import TaskGrid from './components/TaskGrid'
export default {
	 components:{TaskGrid,NewTask,TaskProgress},
     data(){
		 return {
			 task:[]
		 }
	 },
	 watch:{
       task:{
		   deep:true,
		   handler(){
			localStorage.setItem('task',JSON.stringify(this.task));
		   }
	   }
	 },
	 computed:{
		 progress(){
		   const total = this.task.length
		   const done = this.task.filter(t => !t.pending).length
		   return Math.round(done / total * 100)|| 0
		 }
	 },
	 methods:{
		 addTask(task){
			 const sameName = t => t.name === task.name
			 const reallyNew = this.task.filter(sameName).length == 0
			 if(reallyNew){
				 this.task.push({
					 name: task.name,
					 pending: task.pending || true
				 })
			 }
		 },
		 deleteTask(i){
			 this.task.splice(i,1);
		 },
		 toggleTaskState(i){
			 this.task[i].pending = !this.task[i].pending
		 }
	 },
	 created(){
		 const json = localStorage.getItem('task')
		 this.task  = JSON.parse(json) || []
	 }
}
</script>

<style>
	body {
		font-family: 'Lato', sans-serif;
		background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
		color: #FFF;
	}

	#app {
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}

	#app h1 {
		margin-bottom: 5px;
		font-weight: 300;
		font-size: 3rem;
	}
</style>
