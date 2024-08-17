<script setup>
import { ref, onMounted, computed, watch } from "vue";

const todos = ref([]);
const input_content = ref("");

watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  {
    deep: true,
  }
);

const addTodo = () => {
  if (input_content.value.trim() === "") {
    return alert("Please enter task first!");
  }

  todos.value.push({
    content: input_content.value,
    done: false,
    editable: false,
    createdAt: new Date().getTime(),
  });
  input_content.value = "";
};

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>

<template>
  <main class="app">
    <div class="wrapper">
      <section class="greeting">
        <h2 class="title">Hi there,</h2>
      </section>

      <section class="create-todo">
        <h3>CREATE A TODO</h3>

        <form id="new-todo-form" @submit.prevent="addTodo">
          <h4>What's on your todo list?</h4>
          <input
            type="text"
            name="content"
            id="content"
            placeholder="Enter your task"
            v-model="input_content"
          />

          <input type="submit" value="Add todo" />
        </form>
      </section>

      <section class="todo-list">
        <h3>TODO LIST</h3>
        <div class="list" id="todo-list">
          <div
            v-for="todo in todos"
            :class="`todo-item ${todo.done && 'done'}`"
          >
            <label>
              <input type="checkbox" v-model="todo.done" class="done" />
            </label>

            <div class="todo-content">
              <input type="text" v-model="todo.content" />
            </div>

            <div class="actions">
              <button class="delete" @click="removeTodo(todo)">Delete</button>
            </div>
          </div>
        </div>
      </section>
    </div>
  </main>
</template>
