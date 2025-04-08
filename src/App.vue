<template>
  <body dir="rtl">
    <img src="./assets/cover.jpg" class="sea" alt="sea" />
    <div
      style="height: 100%"
      class="style mx-auto w-50 pt-4 align-content-center"
    >
      <app-header></app-header>
      <add-todo @AddNewTodo="HandleAddTodo"></add-todo>
      <div class="d-flex flex-column bg-dark mt-4 p-3 rounded-top">
        <todo-app
          v-for="(item, index) in getTab"
          :key="item.id"
          :todo="item"
          @onDeleted="deleteTodo"
          @changeStatus="changeTodoStatus"
          @dragover.prevent
          @dragstart="dragStart(index)"
          @drop="dragEnd(index)"
        ></todo-app>
        <div class="mt-2 list-unstyled justify-content-between d-flex">
          <ul>
            <li>
              <a href="#" class="text-decoration-none text-light"
                >مقادیر باقی مانده: {{ getActiveTodoCount }}</a
              >
            </li>
          </ul>
          <ul class="gap-4 justify-content-between d-flex">
            <li>
              <a
                href="#"
                id="all"
                :class="{ on: activeTab == 'all' }"
                @click="changeTab('all')"
                >همه</a
              >
            </li>
            <li>
              <a
                href="#"
                id="active"
                :class="{ on: activeTab == 'active' }"
                @click="changeTab('active')"
                >فعال</a
              >
            </li>
            <li>
              <a
                href="#"
                id="completed"
                :class="{ on: activeTab == 'completed' }"
                @click="changeTab('completed')"
                >تکمیل</a
              >
            </li>
          </ul>
          <ul>
            <li>
              <a
                href="#"
                class="text-decoration-none text-light"
                @click="deleteDoTodo"
                >حذف تکمیل ها</a
              >
            </li>
          </ul>
        </div>
        <app-footer></app-footer>
      </div>
    </div>
  </body>
</template>

<script setup>
import AppHeader from "./components/AppHeader.vue";
import AppFooter from "./components/AppFooter.vue";
import AddTodo from "./components/AddTodo.vue";
import TodoApp from "./components/TodoApp.vue";
import {useToast} from "vue-toastification";
import { ref, computed } from "vue";

const todos = ref([]);
const dragging = ref(-1);
const activeTab = ref("all");
const Toast = useToast()

const getActiveTodoCount = computed(() => {
  return todos.value.filter((f) => f.iscomplete == false).length;
});

const getTab = computed(() => {
  switch (activeTab.value) {
    case "all":
      return todos.value;
    case "active":
      return todos.value.filter((f) => f.iscomplete == false);
    case "completed":
      return todos.value.filter((f) => f.iscomplete == true);
    default:
      return todos.value;
  }
});

function HandleAddTodo(title) {
  if (!title) {
    Toast.error(`مقدار را وارد کنید`)
    return;
  }
  const id = Math.random().toString(16).slice(2);
  const todo = { id, title, iscomplete: false };
  todos.value.push(todo);
  Toast.success(`${todo.title} اضافه شد`)
  return todo;
}
function deleteTodo(id) {
  const todo = todos.value.find((f) => f.id == id);
  todos.value = todos.value.filter((f) => f.id !== id);
  Toast.error(`${todo.title} حذف شد`)
  return todo;
}
function changeTodoStatus(id, newStatus) {
  let newTodos = [...todos.value];
  let selectedTodo = newTodos.find((f) => f.id === id);
  selectedTodo.iscomplete = newStatus;
  todos.value = newTodos;
}
function deleteDoTodo() {
  if (confirm("آیا مقادیر انجام شده حذف شود؟")) {
    let newTodos = [...todos.value];
    newTodos = newTodos.filter((f) => f.iscomplete === false);
    todos.value = newTodos;
    Toast.error("عملیات با موفقیت انجام شد")
  }
}
function dragStart(index) {
  dragging.value = index;
}
function dragEnd(index) {
  let newElement = todos.value.splice(dragging.value, 1)[0];
  console.log(newElement);
  console.log(index);
}
function changeTab(tab) {
  activeTab.value = tab;
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body {
  background: rgba(0, 0, 0, 0.541);
  height: 100%;
  width: 100%;
  position: fixed;
  top: 0;
  bottom: 0;
  overflow: auto;
}
.light {
  background: white;
}
.sea {
  top: 0;
  right: 0;
  width: 100%;
  height: 50%;
  z-index: -1;
  position: fixed;
}
svg:hover {
  background: #2260a1;
  border-radius: 100%;
}
button:hover {
  background: #2260a1;
  border-radius: 100%;
}
</style>
