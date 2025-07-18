<template>
  <ul
    role="list"
    class="mt-5 grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-3 mx-4"
  >
    <TransitionRoot as="template" :show="open">
      <Dialog as="div" class="relative z-10" @close="open = false">
        <TransitionChild
          as="template"
          enter="ease-out duration-300"
          enter-from="opacity-0"
          enter-to="opacity-100"
          leave="ease-in duration-200"
          leave-from="opacity-100"
          leave-to="opacity-0"
        >
          <div
            class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"
          />
        </TransitionChild>

        <div class="fixed inset-0 z-10 overflow-y-auto">
          <div
            class="flex min-h-full items-center justify-center p-4 w-full text-center sm:items-center sm:p-0"
          >
            <TransitionChild
              as="template"
              enter="ease-out duration-300"
              enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
              enter-to="opacity-100 translate-y-0 sm:scale-100"
              leave="ease-in duration-200"
              leave-from="opacity-100 translate-y-0 sm:scale-100"
              leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
            >
              <DialogPanel
                class="relative transform overflow-hidden rounded-lg bg-white px-4 pt-5 pb-4 text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg sm:p-6"
              >
                <div class="absolute top-0 right-0 hidden pt-4 pr-4 sm:block">
                  <button
                    type="button"
                    class="rounded-md bg-white text-gray-400 hover:text-gray-500"
                    @click="open = false"
                  >
                    <span class="sr-only">Close</span>
                    <XMarkIcon class="h-6 w-6" aria-hidden="true" />
                  </button>
                </div>
                <div class="">
                  <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left">
                    <DialogTitle
                      as="h3"
                      class="text-lg font-medium leading-6 text-center text-gray-900"
                      >Edit Task</DialogTitle
                    >
                    <div class="mt-4">
                      <label class="text-sm font-medium text-gray-700"
                        >Title</label
                      >
                      <input
                        v-model="title"
                        type="text"
                        class="w-full appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 focus:border-green-500 focus:outline-none focus:ring-green-500 sm:text-sm"
                      />
                      <div class="mt-4">
                        <label
                          for="about"
                          class="mt-4 text-sm font-medium text-gray-700"
                          >Description</label
                        >
                      </div>
                      <textarea
                        v-model="description"
                        rows="3"
                        class="w-full appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 focus:border-green-500 focus:outline-none focus:ring-green-500 sm:text-sm"
                      />
                    </div>
                  </div>
                </div>
                <div class="mt-5 sm:mt-4 sm:flex sm:flex-row-reverse">
                  <button
                    type="button"
                    class="inline-flex w-full justify-center rounded-md border border-transparent bg-green-600 px-4 py-1 text-base font-medium text-white shadow-sm hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-offset-2 sm:ml-3 sm:w-auto sm:text-sm"
                    @click="
                      () => handleEditAction(title, description, selectedId)
                    "
                  >
                    Save
                  </button>
                  <button
                    type="button"
                    class="mt-3 inline-flex w-full justify-center rounded-md border border-gray-300 bg-white px-4 py-1 text-base font-medium text-gray-700 shadow-sm hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-offset-2 sm:mt-0 sm:w-auto sm:text-sm"
                    @click="open = false"
                  >
                    Cancel
                  </button>
                </div>
              </DialogPanel>
            </TransitionChild>
          </div>
        </div>
      </Dialog>
    </TransitionRoot>

    <li
      v-for="task in props.tasks"
      :key="task.id"
      class="col-span-1 divide-y divide-gray-200 rounded-lg bg-neutral-100 border border-gray-300"
    >
      <div class="flex w-full items-center justify-between space-x-6 p-6">
        <div class="flex-1 truncate items-center">
          <div class="flex items-center space-x-3">
            <h3 class="truncate text-sm font-bold text-gray-900">
              {{ task.title }}
            </h3>
            <span
              :class="
                task.is_complete
                  ? 'inline-block flex-shrink-0 rounded-full bg-green-100 px-2 py-0.5 text-xs font-medium text-green-800'
                  : 'inline-block flex-shrink-0 rounded-full bg-yellow-200 px-2 py-0.5 text-xs font-medium text-yellow-800'
              "
              >{{ `${task.is_complete ? "Done" : "To do"}` }}</span
            >
          </div>
          <p class="mt-1 truncate text-sm text-gray-500">
            {{ task.description }}
          </p>
        </div>
      </div>
      <div>
        <div class="-mt-px flex">
          <div class="flex w-0 flex-1">
            <button
              @click="() => handleComplete(task.id, task.is_complete)"
              class="inline-flex flex-1 items-center justify-center py-4 hover:scale-125 transition-transform duration-250"
            >
              <svg
                v-if="task.is_complete"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="green"
                class="w-6 h-6"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M21 16.811c0 .864-.933 1.405-1.683.977l-7.108-4.062a1.125 1.125 0 010-1.953l7.108-4.062A1.125 1.125 0 0121 8.688v8.123zM11.25 16.811c0 .864-.933 1.405-1.683.977l-7.108-4.062a1.125 1.125 0 010-1.953L9.567 7.71a1.125 1.125 0 011.683.977v8.123z"
                />
              </svg>

              <svg
                v-else
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="green"
                class="w-6 h-6"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M4.5 12.75l6 6 9-13.5"
                />
              </svg>
            </button>
          </div>
          <div class="-ml-px flex w-0 flex-1">
            <button
              @click="() => handleEditClick(task)"
              class="inline-flex flex-1 items-center justify-center py-4 hover:scale-125 transition-transform duration-250"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="#666"
                class="w-6 h-6"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M16.862 4.487l1.687-1.688a1.875 1.875 0 112.652 2.652L10.582 16.07a4.5 4.5 0 01-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 011.13-1.897l8.932-8.931zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0115.75 21H5.25A2.25 2.25 0 013 18.75V8.25A2.25 2.25 0 015.25 6H10"
                />
              </svg>
            </button>
          </div>
          <div class="flex w-0 flex-1">
            <button
              @click="() => deleteitem(task.id)"
              class="inline-flex flex-1 items-center justify-center py-4 hover:scale-125 transition-transform duration-250"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="#c00"
                class="w-6 h-6"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M14.74 9l-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 01-2.244 2.077H8.084a2.25 2.25 0 01-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 00-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 013.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 00-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 00-7.5 0"
                />
              </svg>
            </button>
          </div>
        </div>
      </div>
    </li>
  </ul>
</template>

<script setup>
import { ref } from "@vue/reactivity";
import {
  Dialog,
  DialogPanel,
  DialogTitle,
  TransitionChild,
  TransitionRoot,
} from "@headlessui/vue";
import { ExclamationTriangleIcon, XMarkIcon } from "@heroicons/vue/24/outline";
import { useTaskStore } from "../stores/task";
import { supabase } from "../supabase";
import { createToast } from "mosha-vue-toastify";
import "mosha-vue-toastify/dist/style.css";

const toast = (message) => {
  createToast(message, { type: "success" });
};

const props = defineProps({
  tasks: Array,
});

const open = ref(false);
const selectedId = ref("");

const title = ref();
const description = ref("");
const errorMsg = ref(null);

const emit = defineEmits(["handleDelete", "handleEditTask", "handleComplete"]);

const deleteitem = async (id) => {
  toast("The task has been deleted");
  emit("handleDelete", id);
};

const handleComplete = async (id, is_complete) => {
  emit("handleComplete", id, is_complete);
};

const handleEditAction = async (title, description, id) => {
  if (title.length > 3) {
    open.value = false;
    toast("The task has been edited");
    return emit("handleEditTask", title, description, id);
  }
  return alert("The title must be at least 4 characters long");
};

const handleEditClick = (task) => {
  open.value = true;
  selectedId.value = task.id;
  title.value = task.title;
  return (description.value = task.description);
};
</script>

<style scoped></style>