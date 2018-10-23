<template>
  <div class="wrap">
    <h1>Todo With <span>Vue</span>!</h1>
    <TodoHeader :addTodo="addTodo" :allChecked="allChecked" :allActiveTodo="allActiveTodo" :allDoneTodo="allDoneTodo" />
    <TodoBody :todos="todos" :current="current" :removeTodo="removeTodo" :doneTodo="doneTodo" :activeTodo="activeTodo" :saveEditTodo="saveEditTodo" />
    <TodoFooter :total="total" :all="all" :changeCurrentView="changeCurrentView" :removeDoneList="removeDoneList" />
  </div>
</template>

<script>

import TodoHeader from './TodoHeader.vue'
import TodoBody from './TodoBody.vue'
import TodoFooter from './TodoFooter.vue'

export default {
  name: 'Todo',
  components: {
    TodoHeader,
    TodoBody,
    TodoFooter,
  },
  created: function() {
    const STORAGE_KEY = 'todos-storage';
    this.todoStorage = {
      fetch: function () {
        const todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
        return todos;
      },
      save: function (todos) {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(todos))
      }
    };
  },
  mounted: function() {
    this.todos = this.todoStorage.fetch();
    this.total.active = this.countActiveTodo(this.todos);
    this.total.done = this.countDoneTodo(this.todos);
  },
  data() {
    return {
      todoStorage: {},
      todos: [],
      total: {
        active: 0,
        done: 0,
      },
      current: 'all',
    }
  },
  methods: {
    addTodo(todo) {
      this.todos = [...this.todos, { ...todo }];
      this.incrementTotal('active');
    },
    removeTodo(todo) {
      this.todos.filter((item, idx) => {
        if (item.todoId === todo.todoId) {
          this.todos.splice(idx, 1);
        }
      });

      if (todo.done) {
        this.decrementTotal('done');
      } else {
        this.decrementTotal('active');
      }
    },
    incrementTotal(type) {
      this.total[type] += 1;
    },
    decrementTotal(type) {
      if (this.total[type] !== 0) {
        this.total[type] -= 1;
      } else {
        this.total[type] = 0;
      }
    },
    doneTodo(idx) {
      this.todos[idx].done = true;
      this.incrementTotal('done');
      this.decrementTotal('active');
    },
    activeTodo(idx) {
      this.todos[idx].done = false;
      this.incrementTotal('active');
      this.decrementTotal('done');
    },
    saveEditTodo(idx, title) {
      this.todos[idx].title = title;
    },
    changeCurrentView(currentView) {
      this.current = currentView;
    },
    removeDoneList() {
      const doneList = this.todos.filter(todo => {
        return todo.done === true;
      });
      doneList.forEach(todo => {
        this.removeTodo(todo);
      })
    },
    allActiveTodo() {
      this.total.active = this.all;
      this.total.done = 0;
      this.todos.forEach(todo => {
        todo.done = false;
      })
    },
    allDoneTodo() {
      this.total.done = this.all;
      this.total.active = 0;
      this.todos.forEach(todo => {
        todo.done = true;
      })
    },
    countActiveTodo(todos) {
      if (todos) {
        const activeList = todos.filter( item => {
          return item.done === false;
        });
        return activeList.length;
      } else {
        return 0;
      }
    },
    countDoneTodo(todos) {
      if (todos) {
        const doneList = todos.filter( item => {
          return item.done === true;
        });
        return doneList.length;
      } else {
        return 0;
      }
    }
  },
  watch: {
    todos: {
      handler: function (todos) {
        this.todoStorage.save(todos)
      },
      deep: true
    }
  },
  computed: {
    all: function() {
      return this.total.active + this.total.done;
    },
    allChecked: function() {
      if (this.all && this.all === this.total.done) {
        return true;
      }
      return false
    }
  },
}
</script>

<style>
.wrap {position:absolute;top:calc(50% - 360px);left:0;right:0;width:600px;margin:0 auto}
h1 {margin-bottom:50px;font-size:50px}
h1 span {color:#00bc7e}
</style>