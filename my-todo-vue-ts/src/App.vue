<template>
  <div className="app">
    <h1>
      Todoリスト
      <img src="@/assets/favicon.ico" alt="Vue" class="framework-icon" />
    </h1>

    <!-- 入力欄 -->
    <TodoInput v-model="inputText" @add-todo="addTodo" />

    <!-- 未完了 -->
    <h2>未完了のTodo</h2>
    <TodoList
      :todos="todos"
      :isCompleted="false"
      @toggle-todo="toggleTodo"
      @remove-todo="deleteTodo"
    />

    <!-- 完了済み -->
    <h2>完了のTodo</h2>
    <TodoList
      :todos="todos"
      :isCompleted="true"
      @toggle-todo="toggleTodo"
      @remove-todo="deleteTodo"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import TodoInput from "./components/TodoInput.vue";
import TodoList from "./components/TodoList.vue";

// 型定義
export type Todo = {
  id: number;
  text: string;
  completed: boolean;
};

export default defineComponent({
  name: "App",
  components: {
    TodoInput,
    TodoList,
  },
  setup() {
    // 状態管理
    const todos = ref<Todo[]>([]);
    const inputText = ref("");

    //イベント処理
    // Todo追加
    const addTodo = () => {
      // 空文字は追加しない
      if (inputText.value.trim() === "") return;
      // 新しいTodoを作成してリストに追加
      todos.value.push({
        id: todos.value.length + 1,
        text: inputText.value,
        completed: false,
      });
      // 入力欄をクリア
      inputText.value = "";
    };

    // 完了切り替え
    const toggleTodo = (id: number) => {
      todos.value = todos.value.map((todo) =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
      );
    };

    // 削除
    const deleteTodo = (id: number) => {
      todos.value = todos.value.filter((todo) => todo.id !== id);
    };

   

    return {
      todos,
      inputText,
      addTodo,
      toggleTodo,
      deleteTodo,
    };
  },
});
</script>

<style src="./assets/App.css"></style>
