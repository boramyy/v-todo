<template v-cloak>
  <div class="body_todo">
    <h2 v-show="false">Todo List</h2>
    <ul id="todoList">
      <template v-for="(todo, idx) in todos">
        <li
          :class="todo.done ? 'done' : ''"
          :key="todo.todoId"
          v-show="current === 'all' || (current === 'done' ? todo.done === true : todo.done === false)"
        >
          <template v-if="edit.todoId !== todo.todoId">
            <input
              type="checkbox" 
              :id="todo.todoId"
              :name="todo.todoId"
              class="icon-check"
              :checked="todo.done"
              @click="todo.done ? activeTodo(idx) : doneTodo(idx)"
            />
            <label :for="todo.todoId">{{ todo.title }}</label>
            <button class="btn-edit" @click="enterEditMode(todo)">&#9998;</button>
          </template>
          <template v-else>
            <input
              type="checkbox" 
              :id="todo.todoId"
              :name="todo.todoId"
              class="icon-check"
              :checked="todo.done"
              disabled
            />
            <label :for="todo.todoId" v-show="false">{{ todo.title }}</label>
            <input 
              type="text"
              class="input_edit" 
              autofocus
              v-model="edit.title"
              @keyup.enter="saveEdit(idx, edit.title)"
            />
            <button class="btn-save" @click="saveEdit(idx, edit.title)">&#9900;</button>
            <button class="btn-cancel" @click="cancelEdit(todo)">x</button>
            <button class="btn-remove" @click="removeTodo(todo)">&#9003;</button>
          </template>
        </li>
      </template>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'TodoBody',
  data() {
    return {
      edit: {
        todoId: '',
        title: '',
      },
    }
  },
  props: {
    todos: { type: Array, default: () => [] },
    current: { type: String },
    removeTodo: { type: Function, default: () => {}},
    doneTodo: { type: Function, default: () => {}},
    activeTodo: { type: Function, default: () => {}},
    saveEditTodo: { type: Function, default: () => {}},
  },
  methods: {
    enterEditMode(todo) {
      this.edit.todoId = todo.todoId;
      this.edit.title = todo.title;
    },
    exitEditMode() {
      this.edit.todoId = '';
    },
    saveEdit(idx, title) {
      this.saveEditTodo(idx, title);
      this.exitEditMode();
    },
    cancelEdit() {
      this.exitEditMode();
    },

  },
  directives: {
    'todo-focus': function (el, binding) {
      if (binding.value) {
        el.focus();
      }
    }
  }
}
</script>

<style>
[v-cloak] {display:none}
.body_todo {margin:30px 0}
.body_todo #todoList {overflow-x:hidden;overflow-y:scroll;width:100%;max-height:570px;padding:0}
.body_todo #todoList li {height:60px;padding:15px 0;text-align:left;border-bottom:1px dashed #e0e0e0;list-style:none}
.body_todo #todoList li:last-child {border:none}

.icon-check {display:block;float:left;width:30px;height:30px;margin:0 15px 0 0;border:1px solid #ccc;border-radius:50%;-webkit-appearance:none;}
.icon-check:checked {border:6px solid yellowgreen}
.icon-check:not(:checked):hover {border-color:#aaa}

.body_todo #todoList li label {display:block;float:left;width:525px;font-size:24px}
.body_todo #todoList li label:hover {color:#000}
.body_todo #todoList li input:checked + label {color:#ccc;text-decoration:line-through}
.body_todo #todoList li .input_edit {float:left;width:400px;height:50px;margin:-10px 10px -10px -10px;padding:5px 10px;font-size:24px}
.body_todo #todoList li button {float:right;color:#ccc;border:none}
.body_todo #todoList li .btn-edit {padding:0 2px 2px;font-size:22px;transform:scaleX(-1)}
.body_todo #todoList li .btn-edit:hover {color:#95bcd4}
.body_todo #todoList li .btn-save {float:left;padding:0 4px;font-size:26px;line-height:1}
.body_todo #todoList li .btn-cancel {float:left;padding:0 6px 6px;font-size:20px;transform:scale(1.3,1)}
.body_todo #todoList li .btn-save:hover{color:#00c078}
.body_todo #todoList li .btn-cancel:hover{color:#dd5b5b}
.body_todo #todoList li .btn-remove {font-size:22px}
.body_todo #todoList li .btn-remove:hover {color:#dd5b5b}
.body_todo #todoList li label:hover + .btn-remove {color:#da8787}
</style>