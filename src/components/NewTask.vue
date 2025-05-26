<template>
  <div class="space-y-6 max-w-screen-sm mx-4 sm:mx-auto">
    <div class="bg-white px-4 py-5 rounded-lg sm:p-6 border border-gray-300">
      <div class="md:grid md:gap-6">
        <div class="md:col-span-1">
          <h3 class="text-lg font-medium leading-6 text-gray-900">
            Adding New Task
          </h3>
          <p class="text-red-500">{{ errorMsg }}</p>
        </div>
        <div class="mt-5 space-y-6 md:col-span-2 md:mt-0">
          <div class="grid grid-cols-2 gap-6">
            <div class="col-span-3 sm:col-span-2">
              <label class="block text-sm font-medium text-gray-700"
                >Title</label
              >
              <div class="mt-1 flex rounded-md shadow-sm border-black">
                <input
                  v-model="title"
                  type="text"
                  class="w-full appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 focus:border-green-500 focus:outline-none focus:ring-green-500 sm:text-sm"
                />
              </div>
            </div>
          </div>

          <div>
            <label for="about" class="block text-sm font-medium text-gray-700"
              >Description</label
            >
            <div class="mt-1">
              <textarea
                v-model="description"
                rows="3"
                class="w-full appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 focus:border-green-500 focus:outline-none focus:ring-green-500 sm:text-sm"
              />
            </div>
          </div>
          <div class="flex justify-end">
            <button
              @click="addTask()"
              class="ml-3 inline-flex justify-center rounded-md border border-transparent bg-green-600 py-1 px-4 text-sm font-medium text-white shadow-sm hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-offset-2"
            >
              Add task
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "@vue/reactivity";
import { useTaskStore } from "../stores/task";
import { createToast } from "mosha-vue-toastify";
import "mosha-vue-toastify/dist/style.css";

const toast = (message) => {
  createToast(message, { type: "success" });
};

const emit = defineEmits(["handleClick"]);

function addTask() {
  if (title.value.length < 4) {
    errorMsg.value = "The title must be at least 4 characters long";

    return setTimeout(() => {
      return (errorMsg.value = null);
    }, 2000);
  }
  toast("The new task has been added");
  emit("handleClick", title.value, description.value);
  title.value = "";
  description.value = "";
}

const title = ref("");
const description = ref("");
const errorMsg = ref(null);
</script>

<style scoped></style>
