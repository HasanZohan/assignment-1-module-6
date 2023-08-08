<script setup>
import { ref, reactive, computed } from 'vue'

const showLoginForm = ref(true) // To toggle between login and register forms
const registrationSuccess = ref(false);
const loggedIn = ref(false); // Track user login status

const username = ref('')
const password = ref('')
const confirmPassword = ref('')
const registeredUsers = ref([])
const loggedInUser = ref(null)

const register = () => {
  if (username.value.trim() === '' || password.value.trim() === '' || confirmPassword.value.trim() === '') {
    alert('Please fill in all the required fields')
    return; // Stop the registration process
  }

  if (password.value === confirmPassword.value) {
    registeredUsers.value.push({ username: username.value, password: password.value })
    username.value = ''
    password.value = ''
    confirmPassword.value = ''
    showLoginForm.value = true // Show login form after successful registration
    registrationSuccess.value = true; // Set registration success
  } else {
    alert('Passwords do not match')
  }
}

const login = () => {
  const user = registeredUsers.value.find(user => user.username === username.value && user.password === password.value)
  if (user) {
    loggedInUser.value = user
    username.value = ''
    password.value = ''
    showLoginForm.value = false // Hide login form after successful login
    registrationSuccess.value = false; // Reset registration success
  } else {
    alert('Invalid username or password')
  }
}

const logout = () => {
  loggedIn.value = false;
  loggedInUser.value = null;
  username.value = '';
  password.value = '';
  confirmPassword.value = '';
  showLoginForm.value = true;
}

</script>

<template>
  <section class="flex h-screen w-full">
    <div class="w-1/2" style="background-image: url(https://images.unsplash.com/photo-1690555575753-7aa27df96b52?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=774&q=80); background-repeat: no-repeat; background-size: cover;">
      <h1 class="mb-5 text-2xl mt-10 ml-10 text-white">Dev. by Hasan Zohan!</h1>
    </div>
    <div class="w-1/2 flex flex-col justify-center items-center bg-gray-200">
      <h2 class="mb-5 text-xl">{{ loggedInUser ? 'Welcome' : (showLoginForm ? 'Login' : 'Register') }}</h2>
      
      <div class="w-full max-w-xs">
        
        <div v-if="loggedInUser" class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
          <div class="mb-4 text-black-500">
            Hello, {{ loggedInUser.username }}! You are now logged in successfully
          </div>
          <button @click="logout" class="block mx-auto bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="button">
            Logout
          </button>
        </div>
        <!-- Registration form -->
        <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4" v-else-if="!showLoginForm">
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
              Username
            </label>
            <input v-model="username" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="username" type="text" placeholder="Username">
          </div>
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="password">
              Password
            </label>
            <input v-model="password" class="shadow appearance-none border  rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" id="password" type="password" placeholder="******************">
            <!-- <p class="text-red-500 text-xs italic">Please choose a password.</p> -->
          </div>
          <div class="mb-6">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="confirmPassword">
              Confirm Password
            </label>
            <input v-model="confirmPassword" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" id="confirmPassword" type="password" placeholder="******************">
          </div>
          <div class="flex items-center justify-between">
            <button @click="register" class="bg-orange-600 hover:bg-orange-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="button">
              Register
            </button>
            <a @click="showLoginForm = true; registrationSuccess = false;" class="inline-block align-baseline font-bold text-sm text-orange-600 hover:text-orange-800" href="#">
              Back to Login
            </a>
          </div>
        </form>

        <!-- Login form -->
        <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4" v-else>
          <div v-if="registrationSuccess" class="mb-4 text-green-500">
            Registration successful! You can now log in.
          </div>
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
              Username
            </label>
            <input v-model="username" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="username" type="text" placeholder="Username">
          </div>
          <div class="mb-6">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="password">
              Password
            </label>
            <input v-model="password" class="shadow appearance-none border  rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" id="password" type="password" placeholder="******************">
            <!-- <p class="text-red-500 text-xs italic">Please choose a password.</p> -->
          </div>
          <div class="flex items-center justify-between">
            <button @click="login" class="bg-orange-600 hover:bg-orange-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="button">
              Sign In
            </button>
            <a @click="showLoginForm = false" class="inline-block align-baseline font-bold text-sm text-orange-600 hover:text-orange-800" href="#">
              Or Register
            </a>
          </div>
        </form>

        <p class="text-center text-gray-500 text-xs">
          &copy;2020 Acme Corp. All rights reserved.
        </p>
      </div>
    </div>
  </section>
</template>
<style scoped></style>
