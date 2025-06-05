<template lang="pug">
  .todo-app
    header
      h1 Todo App

    form.add-todo(@submit.prevent="addTodo")
      input.todo-input(
        v-model="newTodo"
        type="text"
        placeholder="Add a new todo..."
        required
      )
      button.add-btn(type="submit") Add Todo

    .filters
      button.filter-btn(
        v-for="filter in filters"
        :key="filter"
        @click="currentFilter = filter"
        :class="{ active: currentFilter === filter }"
      ) {{ filter }}

    ul.todo-list
      li.todo-item(
        v-for="todo in filteredTodos"
        :key="todo.id"
      )
        input.todo-checkbox(
          type="checkbox"
          v-model="todo.completed"
        )
        span.todo-text(:class="{ completed: todo.completed }") {{ todo.text }}
        button.delete-btn(@click="deleteTodo(todo.id)") Delete

    footer.todo-footer
      span.todo-count {{ activeTodoCount }} {{ activeTodoCount === 1 ? 'item' : 'items' }} left
      button.clear-completed(
        v-if="completedCount > 0"
        @click="clearCompleted"
      ) Clear completed
  </template>

<script>
export default {
  name: 'TodoApp',
  data() {
    return {
      newTodo: '',
      currentFilter: 'All',
      filters: ['All', 'Active', 'Completed'],
      todos: [],
    }
  },
  computed: {
    filteredTodos() {
      if (this.currentFilter === 'Active') {
        return this.todos.filter((todo) => !todo.completed)
      }
      if (this.currentFilter === 'Completed') {
        return this.todos.filter((todo) => todo.completed)
      }
      return this.todos
    },
    activeTodoCount() {
      return this.todos.filter((todo) => !todo.completed).length
    },
    completedCount() {
      return this.todos.filter((todo) => todo.completed).length
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim()) {
        this.todos.push({
          id: Date.now(),
          text: this.newTodo.trim(),
          completed: false,
        })
        this.newTodo = ''
      }
    },
    deleteTodo(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id)
    },
    clearCompleted() {
      this.todos = this.todos.filter((todo) => !todo.completed)
    },
  },
}
</script>

<style scoped>
.todo-app {
  max-width: 600px;
  margin: 40px auto;
  padding: 20px;
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

header h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 30px;
  font-weight: 300;
  font-size: 2.5rem;
}

.add-todo {
  display: flex;
  margin-bottom: 20px;
  gap: 10px;
}

.todo-input {
  flex: 1;
  padding: 12px 16px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

.todo-input:focus {
  outline: none;
  border-color: #3498db;
}

.add-btn {
  padding: 12px 20px;
  background: #3498db;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.add-btn:hover {
  background: #2980b9;
}

.filters {
  display: flex;
  gap: 5px;
  margin-bottom: 20px;
}

.filter-btn {
  padding: 8px 16px;
  border: 1px solid #ddd;
  background: white;
  cursor: pointer;
  border-radius: 4px;
}

.filter-btn:hover {
  background: #f8f9fa;
}

.filter-btn.active {
  background: #3498db;
  color: white;
  border-color: #3498db;
}

.todo-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  padding: 15px;
  border-bottom: 1px solid #eee;
  gap: 15px;
}

.todo-checkbox {
  width: 18px;
  height: 18px;
}

.todo-text {
  flex: 1;
  font-size: 16px;
}

.todo-text.completed {
  text-decoration: line-through;
  color: #999;
}

.delete-btn {
  padding: 6px 12px;
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.delete-btn:hover {
  background: #c0392b;
}

.todo-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 20px;
  border-top: 1px solid #eee;
  margin-top: 20px;
}

.todo-count {
  color: #666;
  font-size: 14px;
}

.clear-completed {
  background: none;
  border: none;
  color: #e74c3c;
  cursor: pointer;
  font-size: 14px;
  text-decoration: underline;
}

.clear-completed:hover {
  color: #c0392b;
}
</style>
