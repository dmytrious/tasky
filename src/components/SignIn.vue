<template>
  <div class="flex min-h-screen py-10">
    <div
      class="flex flex-1 flex-col justify-center py-12 px-4 sm:px-6 lg:flex-none lg:px-20 xl:px-24"
    >
      <p class="flex justify-center">
        <img class="h-28 w-auto" src="/Logo.png" alt="Tasky Project Logo" />
      </p>

      <div class="mx-auto w-full max-w-sm lg:w-96">
        <div>
          <h2
            class="mt-6 text-3xl font-bold tracking-tight text-gray-900 text-center"
          >
            Log In
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
          <div class="mt-6">
            <form @submit.prevent="signIn" class="space-y-6">
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
                    :type="passwordFieldType"
                    onpaste="return false"
                    placeholder="************"
                    v-model="password"
                    id="password"
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
                  Sign in
                </button>
              </div>
              <div class="text-center text-sm font-semibold">
                Don't have an account ?
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
        src="/MainImg.jpg"
        alt="Tasky Project"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import PersonalRouter from "./PersonalRouter.vue";
import { supabase } from "../supabase";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";

const route = "/auth/sign-up";
const buttonText = "Sign up";

const email = ref("");
const password = ref("");

const errorMsg = ref("");

const passwordFieldType = computed(() =>
  hidePassword.value ? "password" : "text"
);
const hidePassword = ref(true);

const redirect = useRouter();

const signIn = async () => {
  try {
    await useUserStore().signIn(email.value, password.value);
    redirect.push({ path: "/" });
  } catch (error) {
    errorMsg.value = `Error: ${error.message}`;
    setTimeout(() => {
      errorMsg.value = null;
    }, 5000);
  }
};
</script>

<style scoped></style>
