<template>
  <div class="flex min-h-screen py-10">
    <div
      class="flex flex-1 flex-col justify-center py-12 px-4 sm:px-6 lg:flex-none lg:px-20 xl:px-24"
    >
      <div class="mx-auto w-full max-w-sm lg:w-96">
        <div>
          <h2
            class="mt-6 text-3xl font-bold tracking-tight text-gray-900 text-center"
          >
            Registration
          </h2>
          <h1 class="mt-2 text-center">Simplify Your Workflow</h1>
        </div>

        <div class="mt-8">
          <p
            v-if="errorMsg"
            class="text-sm font-semibold text-center text-red-500"
          >
            {{ errorMsg }}
          </p>
          <p
            v-if="successMsg"
            class="text-sm font-semibold text-center text-green-500"
          >
            {{ successMsg }}
          </p>
          <div class="mt-6">
            <form @submit.prevent="signUp" class="space-y-6">
              <div>
                <label
                  for="email"
                  class="block text-sm font-medium text-gray-700"
                  >Your Email address</label
                >
                <div class="mt-1">
                  <input
                    type="email"
                    placeholder="example@mail.com"
                    v-model="email"
                    id="email"
                    required
                    class="block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm focus:border-green-500 focus:outline-none focus:ring-green-500 sm:text-sm"
                  />
                </div>
              </div>

              <div class="space-y-1">
                <label
                  for="password"
                  class="block text-sm font-medium text-gray-700"
                  >Your Password</label
                >
                <div class="mt-1">
                  <input
                    type="password"
                    placeholder="************"
                    v-model="password"
                    id="password"
                    required
                    class="block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm focus:border-green-500 focus:outline-none focus:ring-green-500 sm:text-sm"
                  />
                </div>
              </div>
              <div class="space-y-1">
                <label
                  for="password"
                  class="block text-sm font-medium text-gray-700"
                  >Please Confirm your Password</label
                >
                <div class="mt-1">
                  <input
                    type="password"
                    placeholder="************"
                    v-model="confirmPassword"
                    id="confirmPassword"
                    required
                    class="block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm focus:border-green-500 focus:outline-none focus:ring-green-500 sm:text-sm"
                  />
                </div>
              </div>
              <div>
                <button
                  type="submit"
                  class="flex w-full justify-center rounded-md border border-transparent bg-green-600 py-2 px-4 text-sm font-medium text-white shadow-sm hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-offset-2"
                >
                  Sign up
                </button>
              </div>
              <div class="text-center text-sm font-semibold">
                Have an account already?
                <PersonalRouter :route="route" :buttonText="buttonText" />
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div class="relative hidden sm:block w-0 flex-1">
      <img
        class="absolute inset-0 h-full w-full object-cover rounded-r-xl"
        src="/MainImg.png"
        alt="Tasky Project"
      />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
import PersonalRouter from "./PersonalRouter.vue";
import { useUserStore } from "../stores/user";
import { supabase } from "../supabase";

const route = "/auth/login";
const buttonText = "Log In";

const email = ref(null);
const password = ref(null);
const confirmPassword = ref(null);
const errorMsg = ref(null);
const successMsg = ref(null);

const redirect = useRouter();

async function signUp() {
  if (password.value === confirmPassword.value) {
    try {
      await useUserStore().signUp(email.value, password.value);
      successMsg.value =
        "Please confirm your signup via the link in the email and Log In!";
      setTimeout(() => {
        redirect.push({ path: "/auth/login" });
      }, 3000);
    } catch (error) {
      errorMsg.value = `Error: ${error.message}`;
      setTimeout(() => {
        errorMsg.value = null;
      }, 5000);
    }
    return;
  }
  errorMsg.value = "Passwords do not match";
  setTimeout(() => {
    errorMsg.value = null;
  }, 5000);
}
</script>

<style scoped></style>
