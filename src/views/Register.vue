<template>
  <div>
    <img
      class="mx-auto h-12 w-auto"
      src="https://tailwindui.com/img/logos/workflow-mark-indigo-600.svg"
      alt="Workflow"
    />
    <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">
      Register for free
    </h2>
    <p class="mt-2 text-center text-sm text-gray-600">
      Or
      {{ ' ' }}
      <router-link
        :to="{ name: 'Login' }"
        class="font-medium text-indigo-600 hover:text-indigo-500"
      >
        login to your account
      </router-link>
    </p>

    <form class="mt-8 space-y-6" @submit="register">
      <Alert v-if="errors">
        {{ errors }}
        <span
          @click="errors = null"
          class="w-8 h-8 flex items-center justify-center rounded-full transition-colors cursor-pointer hover:bg-[rgba(0,0,0,0.2)]"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M6 18L18 6M6 6l12 12"
            />
          </svg>
        </span>
      </Alert>

      <input type="hidden" name="remember" value="true" />
      <div class="rounded-md shadow-sm -space-y-px">
        <div class="mb-2">
          <label for="fullname" class="sr-only">Full name</label>
          <input
            id="fullname"
            name="name"
            type="text"
            autocomplete="name"
            required=""
            v-model="user.name"
            class="auth__input"
            placeholder="Full name"
          />
        </div>
        <div>
          <label for="email-address" class="sr-only">Email address</label>
          <input
            id="email-address"
            name="email"
            type="email"
            autocomplete="email"
            required
            v-model="user.email"
            class="auth__input mb-2"
            placeholder="Email address"
          />
        </div>
        <div>
          <label for="password" class="sr-only">Password</label>
          <input
            id="password"
            name="password"
            type="password"
            autocomplete="current-password"
            required=""
            v-model="user.password"
            class="auth__input mb-2"
            placeholder="Password"
          />
        </div>
        <div>
          <label for="password_confirmation" class="sr-only">Password</label>
          <input
            id="password_confirmation"
            name="password_confirmation"
            type="password"
            autocomplete="current-password"
            required=""
            v-model="user.password_confirmation"
            class="auth__input"
            placeholder="Confirm Password"
          />
        </div>
      </div>

      <div>
        <button
          type="submit"
          :disabled="loading"
          class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
          :class="{
            'cursor-not-allowed': loading,
            'hover:bg-indigo-500': loading,
          }"
        >
          <span class="absolute left-0 inset-y-0 flex items-center pl-3">
            <LockClosedIcon
              class="h-5 w-5 text-indigo-500 group-hover:text-indigo-400"
              aria-hidden="true"
            />
          </span>
          <svg
            v-if="loading"
            class="animate-spin -ml-1 mr-3 h-5 w-5 text-white"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
          >
            <circle
              class="opacity-25"
              cx="12"
              cy="12"
              r="10"
              stroke="currentColor"
              stroke-width="4"
            ></circle>
            <path
              class="opacity-75"
              fill="currentColor"
              d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
            ></path>
          </svg>
          Sign up
        </button>
      </div>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { LockClosedIcon } from '@heroicons/vue/solid';
import store from '../store';
import { useRouter } from 'vue-router';
import Alert from '../components/Alert.vue';
import { useToast } from 'vue-toastification';

const toast = useToast();

const router = useRouter();
const user = {
  name: '',
  email: '',
  password: '',
};
const loading = ref(false);
const errors = ref(null);

function register(ev) {
  ev.preventDefault();
  loading.value = true;
  store
    .dispatch('register', user)
    .then(() => {
      loading.value = false;
      router.push({
        name: 'Dashboard',
      });
      toast.success('you have been registred');
    })
    .catch((error) => {
      loading.value = false;
      errors.value = 'invalid input!';
    });
}
</script>
