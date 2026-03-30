<template>
  <!-- input 필드 - 버튼 컴포넌트 -->
  <div class="row mb-3">
    <div class="col">
      <div class="input-group todo-input-group">
        <input
          type="text"
          class="todo-input"
          placeholder="Please Type What to do"
          v-model="todo"
          @keyup.enter="addTodoHandler"
        />
        <!-- 부트스크랩 버튼 -->
        <span class="btn btn-primary" @click="addTodoHandler"> 추가 </span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const todo = ref("");

const emit = defineEmits(["add-todo"]);

// todo 추가하고 입력창 초기화
function addTodoHandler() {
  if (todo.value.length >= 1) {
    console.log("addTodoHandler() 함수 실행");

    emit("add-todo", todo.value);
    todo.value = "";
  }
}
</script>

<style scoped>
.todo-input-group {
  display: flex;
  width: 100%;
}

.todo-input {
  flex: 1;
  min-width: 0;
}
</style>
