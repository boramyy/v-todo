<template>
  <div class="header_todo">
    <h2 v-show="false">Todo Header</h2>
    <div class="box-allcheck">
      <input
        type="checkbox" 
        id="allChecked"
        name="allChecked"
        class="icon-check"
        :checked="allChecked"
        @click="allChecked ? allActiveTodo() : allDoneTodo()"
      />
      <label v-show="false" for="allChecked">Toggle All Checked</label>
    </div>
    <label v-show="false">Todo Write Box</label>
    <input type="text" name="title" placeholder="할 일을 입력하세요!" v-model="todo.title" @keyup.enter="createNewTodo"/>
  </div>
</template>

<script>
export default {
  name: 'TodoHeader',
  props: {
    addTodo: { type: Function, default: () => {} },
    allActiveTodo: { type: Function, default: () => {} },
    allDoneTodo: { type: Function, default: () => {} },
    allChecked: { type: Boolean },
  },
  data() {
    return {
      todo: {
        todoId: '',
        title: '',
        done: false
      }
    }
  },
  methods: {
    createNewId() {
      const date = new Date();
      const id = `${date.getYear()-100}${date.getMonth() + 1}${date.getDate()}_${date.getHours()}_${date.getMinutes()}_${date.getSeconds()}${date.getMilliseconds()}`;
      return id;
    },
    resetNewTodo() {
      this.todo.todoId = '';
      this.todo.title = '';
      this.todo.done = false;
    },
    createNewTodo() {
      if (this.todo.title) {
        this.todo.todoId = this.createNewId();
        this.addTodo(this.todo);
        this.resetNewTodo();
      }
    },
  }
}
</script>

<style>
.header_todo {overflow:hidden;}
.header_todo input[type="text"] {float:right;width:92%;padding:10px 0;font-size:26px;border-top:none;border-right:none;border-bottom:1px solid #ccc;border-left:none}
.header_todo input::placeholder {color:#ccc}
.header_todo .box-allcheck {position:relative;float:left;width:30px;height:30px;margin:10px auto}
.header_todo .box-allcheck .icon-check {float:none}
.header_todo .box-allcheck .icon-check:after {content:'all';position:absolute;top:calc(50% - 7px);right:0;left:0;text-align:center}
</style>