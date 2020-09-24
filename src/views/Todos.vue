<template>
  <div class="home">
    <h1>Todo application</h1>

    <hr>

    <AddTodo @add-todo="addTodo" />

    <select v-model="filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="not-completed">Not completed</option>
    </select>

    <hr>

    <Loader v-if="loading" />

    <TodoList
      v-else-if="filteredTodos.length"
      :todos="filteredTodos"
      @remove-todo="removeTodo"
    />

    <p v-else>No todos!</p>
  </div>
</template>

<script>
import AddTodo from '@/components/AddTodo.vue';
import TodoList from '@/components/TodoList.vue';
import Loader from '@/components/Loader.vue';

export default {
  name: 'Todos',

  data() {
    return {
      todos: [],
      loading: true,
      filter: 'all',
    };
  },

  mounted() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
      .then((response) => response.json())
      .then((result) => {
        setTimeout(() => {
          this.todos = result;
          this.loading = false;
        }, 1000);
        // this.todos = result;
        // this.loading = false;
      });
  },

  // watch: {
  //   filter(value) {
  //     console.log(value);
  //   },
  //   // todos: 'test',
  // },

  components: {
    TodoList,
    AddTodo,
    Loader,
  },

  computed: {
    filteredTodos() {
      if (this.filter === 'all') {
        return this.todos;
      }

      if (this.filter === 'completed') {
        return this.todos.filter((todo) => todo.completed);
      }

      // if (this.filter === 'not-completed') {
      //   return this.todos.filter((todo) => !todo.completed);
      // }

      return this.todos.filter((todo) => !todo.completed);
    },
  },

  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
    },

    addTodo(todo) {
      this.todos.push(todo);
    },

    // test() {
    //   console.log(this.todos);
    // },
  },
};
</script>
