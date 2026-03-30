<template>
  <li
    class="list-group-item todo-row"
    :class="{ 'list-group-item-success': todoItem.completed }"
    @click="emit('toggle-completed', todoItem.id)"
  >
    <div class="todo-left">
      <!-- todo-done 클래스 background-color 변경 - main.css 정의 -->
      <span class="pointer" :class="{ 'todo-done': todoItem.completed }">
        {{ todoItem.todo }}
      </span>

      <!-- 날짜가 있을 때만 표시 -->
      <div v-if="todoItem.dueDate" class="todo-date-text">
        기한: {{ todoItem.dueDate }}
      </div>
    </div>

    <div class="todo-right" @click.stop>
      <!-- 기한 입력 토글 버튼 -->
      <button
        class="btn btn-sm btn-outline-primary due-btn"
        @click="toggleDateInput"
      >
        기한
      </button>

      <!-- 날짜 입력창 토글 -->
      <input
        v-if="showDateInput"
        type="date"
        class="due-date-input"
        v-model="localDueDate"
        @change="saveDueDate"
      />

      <!-- 삭제 버튼 -->
      <span
        class="float-end badge bg-danger pointer delete-btn"
        @click.stop="emit('delete-todo', todoItem.id)"
      >
        삭제
      </span>
    </div>
  </li>
</template>

<script setup>
import { ref } from "vue";

const props = defineProps({
  // 객체(obj)로 전달
  todoItem: {
    type: Object,
    required: true,
  },
});

const emit = defineEmits([
  "delete-todo",
  "toggle-completed",
  "update-due-date",
]);

// 날짜 input 보이기 / 숨기기 용도
const showDateInput = ref(false);

// 기존 날짜가 있으면 기본값으로 보여주기
const localDueDate = ref(props.todoItem.dueDate || "");

// 기한 버튼 누르면 date input 토글
function toggleDateInput() {
  showDateInput.value = !showDateInput.value;
}

/*
날짜를 선택하면
현재 todo의 id와 dueDate를 부모로 전달
*/
function saveDueDate() {
  console.log("saveDueDate() 함수 실행");

  emit("update-due-date", {
    id: props.todoItem.id,
    dueDate: localDueDate.value,
  });

  // 날짜 선택 끝나면 input 닫기
  showDateInput.value = false;
}
</script>

<style scoped>
/* 100% gpt가 작성해줌 - 디자인이 예쁨 */
.todo-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.todo-left {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.todo-right {
  display: flex;
  align-items: center;
  gap: 8px;
}

.todo-date-text {
  font-size: 12px;
  color: #666;
}

.due-date-input {
  width: 140px;
}

.due-btn {
  white-space: nowrap;
}

.delete-btn {
  cursor: pointer;
}
</style>
