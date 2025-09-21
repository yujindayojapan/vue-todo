<template>
  <div class="todo-input-container">
    <input
      v-model="localInput"
      @keyup.enter="onAdd"
      placeholder="入力してください"
    />
    <button @click="onAdd">追加</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from "vue";

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
    const localInput = ref(props.modelValue);

    // 親から値が変わったら同期
    watch(
      () => props.modelValue,
      (newVal) => {
        localInput.value = newVal;
      }
    );

    // 入力が変わったら親に伝える
    watch(localInput, (newVal) => {
      emit("update:modelValue", newVal);
    });

    // Todo 追加イベント
    const onAdd = () => {
      emit("add-todo");
    };

    return {
      localInput,
      onAdd,
    };
  },
});
</script>


