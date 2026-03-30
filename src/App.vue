<template>
  <div id="app" class="container">
    <div class="card card-body bg-light">
      <div class="title">mg_jeong's Todo</div>
    </div>

    <div class="card card-default card-borderless">
      <div class="card-body">
        <!-- !!! 문제 전제조건!!!
              모든 이벤트 처리 App.vue에서 실행!!
        -->
        <InputTodo @add-todo="addTodo" />

        <TodoStatus :cnt="cnt" />

        <!-- 필터 버튼 -->
        <div class="filter-box">
          <button
            class="btn btn-outline-secondary btn-sm"
            :class="{ selected: filterType === 'all' }"
            @click="changeFilter('all')"
          >
            전체
          </button>

          <button
            class="btn btn-outline-secondary btn-sm"
            :class="{ selected: filterType === 'active' }"
            @click="changeFilter('active')"
          >
            진행중
          </button>

          <button
            class="btn btn-outline-secondary btn-sm"
            :class="{ selected: filterType === 'completed' }"
            @click="changeFilter('completed')"
          >
            완료
          </button>
        </div>

        <TodoList
          :todoList="filteredTodoList"
          @delete-todo="deleteTodo"
          @toggle-completed="toggleCompleted"
          @update-due-date="updateDueDate"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import TodoList from "./components/TodoList.vue";
import InputTodo from "./components/InputTodo.vue";
import TodoStatus from "./components/TodoStatus.vue";

// todolist 배열 객체 속성 id로 쓰임
let ts = new Date().getTime();

// 사전 데이터 없이 빈 배열부터 시작
const todoList = ref([]);

// 기본 화면은 진행중 목록
const filterType = ref("active");

/*
cnt를 따로 수동 관리하지 않고
todoList 상태를 기준으로 자동 계산
[전체 개수, 완료 개수, 미완료 개수]
*/
const cnt = computed(() => {
  const total = todoList.value.length;
  const completed = todoList.value.filter((item) => item.completed).length;
  const uncompleted = total - completed;

  return [total, completed, uncompleted];
});

/*
filterType 값에 따라
실제로 화면에 보여줄 목록만 따로 계산
*/
const filteredTodoList = computed(() => {
  if (filterType.value === "all") {
    return todoList.value;
  } else if (filterType.value === "active") {
    return todoList.value.filter((item) => item.completed === false);
  } else if (filterType.value === "completed") {
    return todoList.value.filter((item) => item.completed === true);
  }
});

function changeFilter(type) {
  filterType.value = type;
}

/*
적용점
- todo 문자열만 전달받음
- 날짜는 나중에 각 리스트 아이템에서 따로 입력
*/
function addTodo(todo) {
  console.log("addTodo() 함수 실행");

  if (todo.length >= 1) {
    todoList.value.push({
      id: new Date().getTime(),
      todo,
      completed: false,
      dueDate: "",
    });
  }
}

// 자식에서 id값(int형) 그대로 받음
function deleteTodo(id) {
  console.log("deleteTodo() 함수 실행");

  const i = todoList.value.findIndex((t) => t.id === id);
  if (i === -1) return;

  todoList.value.splice(i, 1);
}

// 자식에서 id값(int형) 그대로 받음
function toggleCompleted(id) {
  console.log("toggleCompleted() 함수 실행");

  const i = todoList.value.findIndex((t) => t.id === id);
  if (i === -1) return;

  // completed 값 반전
  todoList.value[i].completed = !todoList.value[i].completed;
}

/*
각 todo마다 기한 날짜를 따로 저장
TodoListItem에서 { id, dueDate } 형태로 emit 해줌
*/
function updateDueDate(todoData) {
  console.log("updateDueDate() 함수 실행");

  const { id, dueDate } = todoData;

  const i = todoList.value.findIndex((t) => t.id === id);
  if (i === -1) return;

  todoList.value[i].dueDate = dueDate;
}
</script>

<style scoped>
.filter-box {
  display: flex;
  gap: 8px;
  margin: 8px 0 14px 0;
}

.selected {
  background-color: #dfe8e3;
  border-color: #9db3a5;
}
</style>
