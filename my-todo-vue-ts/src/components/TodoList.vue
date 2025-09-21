<template>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id" class="todo-item">
      <span class="todo-text">{{ todo.text }}</span>
      <button @click="$emit('toggle-todo', todo.id)">
        {{ isCompleted ? "未完了" : "完了" }}
      </button>
      <button @click="$emit('remove-todo', todo.id)">削除</button>
    </li>
  </ul>
</template>

<script lang="ts">
import { defineComponent, computed } from "vue";
import type { Todo } from "../App.vue";

export default defineComponent({
  name: "TodoList",
  props: {
    todos: {
      type: Array as () => Todo[],
      required: true,
    },
    isCompleted: {
      type: Boolean,
      required: true,
    },
  },
  emits: ["toggle-todo", "remove-todo"],
  setup(props) {
    // フィルタリング
    const filteredTodos = computed(() =>
      props.todos.filter((todo) => todo.completed === props.isCompleted)
    );

    return {
      filteredTodos,
    };
  },
});
</script>


