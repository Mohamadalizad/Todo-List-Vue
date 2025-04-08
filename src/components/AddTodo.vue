<template>
  <div class="relative mt-4">
    <input
      v-model="todoTitle"
      type="text"
      class="py-4 px-5 form-control relative"
      name=""
      id=""
      placeholder="افزودن ویژگی جدید..."
      @keypress.enter="AddTodo"
    />
    <svg
      @click="AddTodo"
      xmlns="http://www.w3.org/2000/svg"
      fill="none"
      viewBox="0 0 24 24"
      stroke-width="1.5"
      stroke="currentColor"
      class="abs"
    >
      <path
        stroke-linecap="round"
        stroke-linejoin="round"
        d="M12 9v6m3-3H9m12 0a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"
      />
    </svg>
  </div>
</template>

<script setup >
import { ref, defineEmits } from "vue";

// create todoTitle variable
const todoTitle = ref("");
const emits = defineEmits("AddNewTodo");

// AddTodo function
function AddTodo() {
  emits("AddNewTodo", todoTitle.value);
  todoTitle.value = "";
}
</script>
<style scoped></style>;
