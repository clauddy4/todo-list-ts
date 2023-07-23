<template>
  <div id="app">
    <AppHeader />
    <AppFilters :active-filter="activeFilter" @setFilter="setFilter" />
    <main class="app-main">
      <AppTodoList :todos="filteredTodos" @toggle-todo="toggleTodo" @remove-todo="removeTodo" />
      <AppAddTodo @addTodo="addTodo" />
    </main>
    <AppFooter :stats="stats" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import AppHeader from '@/components/AppHeader.vue';
import AppFilters from '@/components/AppFilters.vue';
import AppTodoList from '@/components/AppTodoList.vue';
import AppAddTodo from '@/components/AppAddTodo.vue';
import AppFooter from '@/components/AppFooter.vue';
import { Todo } from '@/types/Todo';
import { Filter } from '@/types/Filter';
import {Stats} from "@/types/Stats";

interface State {
  todos: Todo[];
  activeFilter: Filter;
}

export default defineComponent({
  components: {
    AppFooter,
    AppAddTodo,
    AppTodoList,
    AppHeader,
    AppFilters,
  },
  data: (): State => ({
    todos: [
      { id: 0, text: 'Learn', completed: true },
      { id: 1, text: 'Learn 2', completed: false },
      { id: 2, text: 'Learn 3', completed: false },
    ],
    activeFilter: 'All',
  }),
  computed: {
    filteredTodos(): Todo[] {
      switch (this.activeFilter) {
        case 'Active':
          return this.activeTodos;
        case 'Done':
          return this.doneTodos;
        case 'All':
        default:
          return this.todos;
      }
    },
    activeTodos(): Todo[] {
      return this.todos.filter((todo) => !todo.completed)
    },
    doneTodos(): Todo[] {
      return this.todos.filter((todo) => todo.completed)
    },
    stats(): Stats {
      return {
        active: this.activeTodos.length,
        done: this.doneTodos.length
      }
    }
  },
  methods: {
    toggleTodo(id: number) {
      const targetTodo = this.todos.find((todo: Todo) => todo.id === id);
      if (targetTodo) targetTodo.completed = !targetTodo.completed;
    },
    removeTodo(id: number) {
      this.todos = this.todos.filter((todo: Todo) => todo.id !== id);
    },
    addTodo(todo: Todo) {
      this.todos.push(todo);
    },
    setFilter(filter: Filter) {
      this.activeFilter = filter;
    },
  },
});
</script>
