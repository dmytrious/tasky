<template>
  <div>
    <NavBar/>
    <NewTask
      @handleClick="handleClick"
    />
    <TaskItem
      @handleDelete="handleDelete"
      @handleEditTask="handleEditTask"
      @handleComplete="handleComplete"
      :tasks="arrayTask"
    />
  </div>
</template>

<script setup>
import NavBar from "../components/Nav.vue"
import NewTask from "../components/NewTask.vue";
import TaskItem from "../components/TaskItem.vue";
import { supabase } from "../supabase"
import { useTaskStore } from "../stores/task";
import { ref } from "vue";
import { createToast } from "mosha-vue-toastify"
import "mosha-vue-toastify/dist/style.css"

const toast = (message) => {
  createToast(message, { type: "success" })
}

const arrayTask = ref(null);

async function getTask() {
  arrayTask.value = await useTaskStore().fetchTasks();
  console.log(arrayTask);
}

getTask();

const handleDelete = async(id) =>{
  try{
    const {data, error} = await supabase
    .from('tasks')
    .delete()
    .match({id:id})

    getTask()
  }catch(error){
    console.log("this is my error:", error)
  }
}

const handleComplete = async (id, is_complete) =>{
  await useTaskStore().taskComplete(id, is_complete)
  toast(is_complete ? "The task status has been changed to incompleted" : "The task has been completed", {type: "success"})
  getTask()
}

const handleEditTask = async (title, description, id) =>{
  await useTaskStore().editTask(title,description, id)
  getTask()
}

const handleClick = async (title, description) => {
  await useTaskStore().addTask(title, description);
  getTask();
};
</script>

<style scoped></style>