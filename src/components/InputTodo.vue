<template>
  <!-- input 필드 - 버튼 컴포넌트 -->
  <div class="row mb-3">
    <div class="col">
      <div class="input-group">
        <input
          type="text"
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

<script>
export default {
  name: "InputTodo",
  data() {
    return {
      todo: "",
    };
  },
  //이벤트 이름 명시
  emits: ["add-todo"],

  //todo 추가하고 입력창 초기화 안되는 문제
  // this.todo = "";
  // 핸들링 함수 마지막에 문자열 초기화하기
  methods: {
    addTodoHandler() {
      if (this.todo.length >= 1) {
        console.log("addTodoHandler() 함수 실행");

        this.$emit("add-todo", this.todo);
        this.todo = "";
      }
    },
  },
};
</script>
