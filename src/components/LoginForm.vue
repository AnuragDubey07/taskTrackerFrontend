<template>
  <div>
    <h2 id="heading">Login</h2>
    <form id="loginform" @submit.prevent="loginUser">
     
      <div>
        <label id="loginInput" for="username">Email:</label>
        <input class="inputfield" type="text" id="email" v-model="email" required />
      </div>
      <div>
        <label for="password">Password:</label>
        <input class="inputfield" type="password" id="password" v-model="password" required />
      </div>
      <button id="loginBtn" type="submit">Login</button>
    </form>

    <!-- <TaskList v-if="loginSuccess" :tasks="tasks" @add-task="addTask" @complete-task="completeTask" /> -->
  </div>
</template>

<script>
import { mapActions } from 'vuex';
import axios from 'axios'; // Import axios here
// import TaskList from './TaskList.vue';

export default {
 
  // components: {
  //   TaskList,
  // },
  data() {
    return {
      username: '',
      password: '',
      email:'',
      loginSuccess: false,
      tasks: [],
    };
  },
  methods: {
    ...mapActions(['loginUser']),

    async loginUser() {
      try {
        const credentials = {  password: this.password,email:this.email };
        const response = await axios.post('http://localhost:3000/users/login',
         credentials
          
        );
       // const credentials = {  password: this.password,email:this.email };
        //await this.loginUser(credentials);
        console.log(response);

        // Set loginSuccess to true when login is successful
        this.loginSuccess = true;

        // Retrieve user tasks after successful login
        // this.tasks = await this.fetchUserTasks();

        // Redirect to LoginSuccessful component after successful login
       this.$router.push("/task-list");
      } catch (error) {
        console.error("Login failed", error);
        // Handle login failure
      }
    },

    async fetchUserTasks() {
      // Call the API to fetch the user's tasks
      const response = await axios.get(`/api/users/${this.username}/tasks`);
      return response.data.tasks || [];
    },

    addTask(newTask) {
    
      this.tasks.push(newTask);

     
      axios.post(`/api/users/${this.username}/add-task`, { task: newTask });
    },

    completeTask(index) {
     
      this.tasks.splice(index, 1);

      // Call the API to complete the task for the user
      axios.post(`/api/users/${this.username}/complete-task/${index}`);
    },
  },
};
</script>

<style>

#loginform {
    max-width: 300px;
    margin: auto;
  }
  #heading{
    color:#4ba3fb;
    font-size: 2.5vmax;
    padding: 2vmax;
  }

  label {
    display: block;
    margin-bottom: 8px;
    border-width: 1;
    border-color: black;
  }

  #loginInput {
    width: 100%;
    padding: 8px;
    margin-bottom: 16px;
  }

  #loginBtn {
    background-color: #4caf50;
    color: white;
    padding: 10px 15px;
    border: none;
    cursor: pointer;
  }
  .inputfield{
    padding: 10px;
    background-color: red;
  }
  #loginform input {
	display: block;
	width: 100%;
	font-size: 1.125rem;
	padding: 1rem 1.5rem;
	color: var(--dark);
	background-color: #ffffff;
	border-radius: 0.5rem;
	box-shadow: var(--shadow);
	margin-bottom: 1.5rem;
}
</style>

