<template>
  <div id="app">
    <section class="todoapp">
      <header class="header">
        <h1>ToDo リスト</h1>
        <input
          class="new-todo"
          placeholder="追加する ToDo を入力してください"
          autofocus
          autocomplete="off"
          v-model="newTodo"
          @keyup.enter="addTodo"
        />
      </header>
      <section class="main">
        <ul class="todo-list">
          <li
            v-for="todo in filteredTodos"
            class="todo"
            :key="todo.id"
            :class="{completed: todo.completed}"
          >
            <div class="view">
              <label>
                <input class="toggle" type="checkbox" v-model="todo.completed" />
                {{ todo.title }}
              </label>
              <button class="remove" @click="removeTodo(todo)">削除</button>
            </div>
          </li>
        </ul>
      </section>
      <footer class="footer">
        <span class="todo-count">
          <strong>{{ filteredTodos.length }}</strong> 件を表示
        </span>
        <label>
          <input class="radio" type="radio" v-model="visibility" value="all"/>すべて
        </label>
        <label>
          <input class="radio" type="radio" v-model="visibility" value="active" />作業中
        </label>
        <label>
          <input class="radio" type="radio" v-model="visibility" value="completed" />完了
        </label>
      </footer>
    </section>
  </div>
</template>

<script>
let filters = {
  all: function(todos) {
    return todos;
  },
  active: function(todos) {
    return todos.filter(function(todo) {
      return !todo.completed;
    });
  },
  completed: function(todos) {
    return todos.filter(function(todo) {
      return todo.completed;
    });
  }
};

export default {
  name: "app",
  components: {},
  data: function() {
    return {
      todos: [],
      newTodo: "",
      uid: 0,
      visibility: "all"
    };
  },

  // https://jp.vuejs.org/v2/api/#computed
  computed: {
    filteredTodos: function() {
        return filters[this.visibility](this.todos);
    }
  },

  // https://jp.vuejs.org/v2/api/#methods
  methods: {
    addTodo: function() {
      let value = this.newTodo && this.newTodo.trim();
      if (!value) {
        return;
      }
      this.todos.push({
        id: this.uid++,
        title: value,
        completed: false
      });
      this.newTodo = "";
    },
    removeTodo: function(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1);
    }
  }
};
</script>

<style>
</style>
