<template>
    <div>
      <!-- <h2>Task List</h2>
    
      <form @submit.prevent="addTask">
        <label for="newTask">New Task:</label>
        <input type="text" id="newTask" v-model="newTask" required />
        <button type="submit">Add Task</button>
      </form> -->
      <section class="create-todo">
			<h3>CREATE A TASK</h3>

			<form id="new-todo-form" @submit.prevent="addTask">
				<h4>What's on your task list?</h4>
				<input 
					type="text" 
					name="content" 
					id="newTask" 
					placeholder="Enter your task to be done."
					v-model="newTask" required />
				
				

				<input type="submit" value="Add Task" />
			</form>
		</section>

    <section class="todo-list">
			<h3>YOUR TASKS</h3>
			<div class="list" id="todo-list">

				<div v-for="(task, index) in tasks" :key="index" :class="`todo-item `">
				
					<div class="todo-content">
						<!-- <input type="text" v-model="todo.content" /> -->
            
            {{ task }}
					</div>


					<div class="actions">
						<button class="delete" @click="removeTodo(task)">Delete</button>
					</div>
				</div>

			</div>
		</section>
      <!-- <ul>
        <li v-for="(task, index) in tasks" :key="index">
          {{ task }}
          <button @click="completeTask(index)">Complete</button>
        </li>
      </ul> -->
    </div>
  </template>
  
  <script>
  // Remove the unused import statement
  export default {
    data() {
      return {
        newTask: '',
        tasks: [],
      };
    },
    methods: {
      addTask() {
        if (this.newTask.trim() !== '') {
          this.tasks.push(this.newTask.trim());
          this.newTask = '';
        }
      },
      removeTodo (todo) {
      this.tasks = this.tasks.filter((t) => t !== todo)
}

    },
  };
  </script>
  