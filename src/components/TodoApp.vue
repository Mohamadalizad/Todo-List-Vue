<template>
  <div style="cursor: move" class="d-flex justify-content-between">
    <span class="text-white form-check">
      <input
        @click="changeStatus"
        :checked="todo.iscomplete ? true : null"
        type="checkbox"
        class="form-check-input rounded-circle p-3"
      />
      <p class="form-check-label p-2">
        <span v-if="todo.iscomplete == true"> {{ todo.title }} </span>
        <span v-else v-text="todo.title"></span>
      </p>
    </span>
    <span>
      <svg
        @click="deleteTodo"
        xmlns="http://www.w3.org/2000/svg"
        style="height: 2rem; width: 2rem"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
        class="text-white"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="m9.75 9.75 4.5 4.5m0-4.5-4.5 4.5M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"
        />
      </svg>
    </span>
  </div>
  <hr class="text-white" />
</template>

<script setup>
import { defineProps, toRef, defineEmits } from "vue";
// todo : Object

// use Object prop
const props = defineProps({
  todo: Object,
});
const emits = defineEmits(["onDeleted", "changeStatus"]);

const todo = toRef(props, "todo");

// delete my todo function
function deleteTodo() {
  if (confirm("آیا از حذف خود اطمینان دارید؟")) {
    emits("onDeleted", todo.value.id);
  }
}

// update my todo function 
function changeStatus() {
  emits("changeStatus", todo.value.id, !todo.value.iscomplete);
}
</script>