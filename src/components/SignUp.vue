
<template>
  <div>
    <h2 style="color: #333;">Signup</h2>
    <form id="loginform" @submit.prevent="signup" style="max-width: 300px; margin: auto;">
      <div style="margin-bottom: 15px;">
        <label for="username">Username:</label>
        <input v-model="username" type="text" id="username" required style="width: 100%; padding: 8px;" />
      </div>
      <div>
        <label id="loginInput"  for="username">Email:</label>
        <input class="inputfield" type="text" id="email" v-model="email" required />
      </div>
      <div style="margin-bottom: 15px;">
        <label for="password">Password:</label>
        <input v-model="password" type="password" id="password" required style="width: 100%; padding: 8px;" />
      </div>
      <div style="margin-bottom: 15px;">
        <label for="confirmPassword">Confirm Password:</label>
        <input v-model="confirmPassword" type="password" id="confirmPassword" required style="width: 100%; padding: 8px;" />
      </div>
      <button type="submit" style="background-color: #4CAF50; color: white; padding: 10px; border: none; cursor: pointer;">Signup</button>
    </form>
    <div v-if="signupStatus" style="margin-top: 15px; color: #4CAF50;">
      {{ signupStatus }}
      <!-- Redirect to the RegistrationSuccess page after successful signup -->
      <router-link v-if="signupStatus === 'Successfully signed up!'" to="/registration-success" style="color: #4CAF50;">View Registration Details</router-link>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';

export default {
  setup() {
    const username = ref('');
    const password = ref('');
    const email = ref('');
    const confirmPassword = ref('');
    const signupStatus = ref(null);
    const router = useRouter();

    const signup = async () => {
      if (password.value !== confirmPassword.value) {
        signupStatus.value = 'Passwords do not match';
        return;
      }

      try {
        const response = await axios.post('http://localhost:3000/users/register', {
          name: username.value,
          password: password.value,
          email: email.value,
          
        });
        const key = "token";    //token save to local storage.
        signupStatus.value = response.data.message; // Assuming your backend sends a success message
        localStorage.setItem(key, JSON.stringify(response.data.token));
        // console.log(response);
        router.push("/login");
      } catch (error) {
        signupStatus.value = 'Signup failed. Please try again.';
        console.error('Signup Error:', error);
      }
    };

    return { username, password, confirmPassword, signupStatus, signup ,email};
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

