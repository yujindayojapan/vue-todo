<template>
  <ul>
    <li
      v-for="todo in filteredTodos"
      :key="todo.id"
      :class="['todo-item', { 'completed-todo': todo.completed }]"
    >
      <span class="todo-text">{{ todo.text }}</span>
      <div class="button-group">
        <button
          class="todo-btn toggle-btn"
          :class="{ uncomplete: todo.completed }"
          @click="$emit('toggle-todo', todo.id)"
        >
          {{ todo.completed ? "未完了" : "完了" }}
        </button>
        <button
          class="todo-btn delete-btn"
          @click="$emit('remove-todo', todo.id)"
        >
          削除
        </button>
      </div>
    </li>
  </ul>
</template>

<script lang="ts">
import { defineComponent, computed, type PropType } from "vue";
import type { Todo } from "../App.vue";

export default defineComponent({
  name: "TodoList",
  props: {
    todos: {
      type: Array as PropType<Todo[]>,
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
