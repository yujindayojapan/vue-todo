<template>
  <div class="todo-input-container">
    <input
      v-model="inputText"
      @keyup.enter="onAdd"
      placeholder="入力してください"
    />
    <button @click="onAdd">追加</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, computed } from "vue";

export default defineComponent({
  name: "TodoInput",
  props: {
    modelValue: {
      type: String,
      required: true,
    },
  },
  emits: ["update:modelValue", "add-todo"],
  setup(props, { emit }) {
    // 双方向バインディング用の computed
    const inputText = computed({
      get: () => props.modelValue,
      set: (val: string) => emit("update:modelValue", val),
    });

    // Todo 追加イベント
    const onAdd = () => {
      emit("add-todo");
    };

    return {
      inputText,
      onAdd,
    };
  },
});
</script>
