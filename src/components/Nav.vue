<template>
  <Popover as="template" v-slot="{ open }">
    <header
      :class="[
        open ? 'fixed inset-0 z-40 overflow-y-auto' : '',
        'bg-white lg:static lg:overflow-y-visible',
      ]"
    >
      <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 py-3">
        <div
          class="relative flex justify-between lg:gap-8 xl:grid xl:grid-cols-12"
        >
          <div
            class="flex md:absolute md:inset-y-0 md:left-0 lg:static xl:col-span-2"
          >
            <div class="flex flex-shrink-0 items-center">
              <a href="#">
                <img
                  class="block h-12 w-auto"
                  src="/Logo.png"
                  alt="Tasky Project Logo"
                />
              </a>
            </div>
          </div>
          <div class="min-w-0 flex-1 md:px-8 lg:px-0 xl:col-span-6">
            <div
              class="flex items-center px-6 py-4 md:mx-auto md:max-w-3xl lg:mx-0 lg:max-w-none xl:px-0"
            >
              <div class="w-full">
                <label for="search" class="sr-only">Search</label>
                <div class="relative"></div>
              </div>
            </div>
          </div>
          <div class="lg:flex lg:items-center lg:justify-end xl:col-span-4">
            <p class="mb-2 sm:mt-2">Welcome <b>{{user?.email?.split("@")[0]}}</b></p>
            <button
              @click="handleLogOut()"
              href="#"
              class="ml-6 inline-flex items-center rounded-md border border-transparent bg-green-600 px-4 py-1.5 text-sm font-medium text-white shadow-sm hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-offset-2"
            >
              Log Out
            </button>
          </div>
        </div>
      </div>

      <PopoverPanel as="nav" class="lg:hidden" aria-label="Global">
        <div class="border-t border-gray-200 pt-4 pb-3">
          <div class="mx-auto flex max-w-3xl items-center px-4 sm:px-6">
            <div class="flex-shrink-0">
              <img class="h-10 w-10 rounded-full" :src="user.imageUrl" alt="" />
            </div>
            <button
              type="button"
              class="ml-auto flex-shrink-0 rounded-full bg-white p-1 text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-offset-2"
            >
              <span class="sr-only">View notifications</span>
            </button>
          </div>
        </div>
      </PopoverPanel>
    </header>
  </Popover>
</template>

<script setup>
import {
  Menu,
  MenuButton,
  MenuItem,
  MenuItems,
  Popover,
  PopoverButton,
  PopoverPanel,
} from "@headlessui/vue"
import { useUserStore } from "../stores/user"
import { supabase } from "../supabase"
import { ref } from "vue"

const user = ref("")

const fetchUser = async () => {
  user.value = await supabase.auth.user()
}

fetchUser()

console.log("hehe", user)
const handleLogOut = async () => {
  await supabase.auth.signOut()
  return window.location.reload()
}
</script>

<style scoped></style>
