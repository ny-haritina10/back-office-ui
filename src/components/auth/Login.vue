<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-100 py-8 px-4 sm:px-6 lg:px-8">
    <div class="w-full max-w-sm space-y-6 bg-white rounded-lg shadow-lg border border-gray-200 p-6">
      <!-- Logo and Header -->
      <div class="text-center">
        <div class="flex justify-center">
          <div class="h-12 w-12 rounded-full bg-primary flex items-center justify-center">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-white" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M2 17a5 5 0 0 0 10 0c0-2.76-2.5-5-5-3-2.5-2-5 .24-5 3Z"/>
              <path d="M12 17a5 5 0 0 0 10 0c0-2.76-2.5-5-5-3-2.5-2-5 .24-5 3Z"/>
              <path d="M7 14c3.22-2.91 4.29-8.75 5-12 1.66 2.38 4.94 9 5 12"/>
              <path d="M22 9c-4.29 1.33-7.12 2.67-12 8"/>
            </svg>
          </div>
        </div>
        <h2 class="mt-4 text-2xl font-bold text-gray-800">Login Form</h2>
        <p class="mt-1 text-sm text-gray-600">
          Don't have an account?
          <button 
            @click="toggleForm" 
            class="font-medium text-primary hover:text-primary-dark transition-colors"
          >
            Sign up
          </button>
        </p>
      </div>
      
      <!-- Login Form -->
      <form class="space-y-5" @submit.prevent="handleLogin">
        <div class="space-y-4">
          <div>
            <label for="email-address" class="block text-sm font-medium text-gray-700 mb-1">Email</label>
            <div class="relative">
              <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <rect x="2" y="4" width="20" height="16" rx="2" />
                  <path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7" />
                </svg>
              </div>
              <input 
                id="email-address" 
                name="email" 
                type="email" 
                autocomplete="email" 
                required 
                v-model="email"
                class="pl-10 appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-primary focus:border-primary text-sm" 
                placeholder="you@example.com"
              />
            </div>
            <p v-if="errors.email" class="mt-1 text-xs text-red-600">{{ errors.email }}</p>
          </div>
          <div>
            <div class="relative">
              <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <rect x="3" y="11" width="18" height="11" rx="2" ry="2"/>
                  <path d="M7 11V7a5 5 0 0 1 10 0v4"/>
                </svg>
              </div>
              <input 
                id="password" 
                name="password" 
                type="password" 
                autocomplete="current-password" 
                required 
                v-model="password"
                class="pl-10 appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-primary focus:border-primary text-sm" 
                placeholder="••••••••"
              />
            </div>
            <p v-if="errors.password" class="mt-1 text-xs text-red-600">{{ errors.password }}</p>
          </div>
        </div>

        <div>
          <button 
            type="submit" 
            :disabled="isLoading"
            class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-primary hover:bg-primary-dark focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-primary transition duration-150 ease-in-out"
          >
            <span v-if="isLoading" class="absolute left-0 inset-y-0 flex items-center pl-3">
              <svg class="animate-spin h-4 w-4 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
              </svg>
            </span>
            <span v-else class="absolute left-0 inset-y-0 flex items-center pl-3">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-white" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M15 3h4a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2h-4"/>
                <polyline points="10 17 15 12 10 7"/>
                <line x1="15" y1="12" x2="3" y2="12"/>
              </svg>
            </span>
            {{ isLoading ? 'Signing in...' : 'Sign in' }}
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue';

// Form data
const email = ref('');
const password = ref('');
const rememberMe = ref(false);
const isLoading = ref(false);
const errors = reactive({
  email: '',
  password: ''
});

// Emit events for parent component
const emit = defineEmits(['login', 'toggle-form']);

// Form validation
const validateForm = () => {
  let isValid = true;
  errors.email = '';
  errors.password = '';

  // Email validation
  if (!email.value) {
    errors.email = 'Email is required';
    isValid = false;
  } else if (!/^\S+@\S+\.\S+$/.test(email.value)) {
    errors.email = 'Please enter a valid email address';
    isValid = false;
  }

  // Password validation
  if (!password.value) {
    errors.password = 'Password is required';
    isValid = false;
  } else if (password.value.length < 6) {
    errors.password = 'Password must be at least 6 characters';
    isValid = false;
  }

  return isValid;
};

// Handle login submission
const handleLogin = async () => {
  if (!validateForm()) return;
  
  isLoading.value = true;
  
  try {
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 1500));
    
    // Emit login event with form data
    emit('login', {
      email: email.value,
      password: password.value,
      rememberMe: rememberMe.value
    });
    
    // Reset form
    email.value = '';
    password.value = '';
    rememberMe.value = false;
  } catch (error) {
    console.error('Login error:', error);
  } finally {
    isLoading.value = false;
  }
};

// Toggle between login and signup forms
const toggleForm = () => {
  emit('toggle-form');
};
</script>

<style scoped>
:root {
  --color-primary: 25 95 125;
  --color-primary-dark: 20 75 100;
}

.bg-primary {
  background-color: rgb(var(--color-primary));
}

.text-primary {
  color: rgb(var(--color-primary));
}

.hover\:text-primary-dark:hover {
  color: rgb(var(--color-primary-dark));
}

.focus\:ring-primary:focus {
  --tw-ring-color: rgb(var(--color-primary));
}

.focus\:border-primary:focus {
  border-color: rgb(var(--color-primary));
}

.hover\:bg-primary-dark:hover {
  background-color: rgb(var(--color-primary-dark));
}
</style>