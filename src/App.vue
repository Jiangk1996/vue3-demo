<template>
  <section class="todoapp">
    <header class="header">
      <h1>todos</h1>
      <input
        autofocus="autofocus"
        autocomplete="off"
        placeholder="What needs to be done?"
        class="new-todo"
        v-model="newTodo"
        @keyup.enter="addTodo"
      />
    </header>
    <section class="main" v-show="todos.length">
      <input
        id="toggle-all"
        type="checkbox"
        class="toggle-all"
        v-model="allDone"
      />
      <label for="toggle-all">Mark all as complete</label>
      <ul class="todo-list">
        <li v-for="todo in todos" :key="todo.id" class="todo" :class="{completed: todo.completed}">
          <div class="view">
            <input type="checkbox" class="toggle" v-model="todo.completed" />
            <label>{{ todo.title }}</label>
            <button class="destory"></button>
          </div>
        </li>
      </ul>
    </section>
    <footer class="footer" v-show="todos.length">
      <span class="todo-count"
        ><strong>{{ remaining }}</strong> item left
      </span>
      <button
        class="clear-completed"
        @click="removeCompleted"
        v-show="todos.length > remaining"
      >
        Clear completed
      </button>
    </footer>
  </section>
</template>

<script>
import { ref, reactive, toRefs, computed } from 'vue'

export default {
  name: 'App',
  setup() {
    const state = reactive({
      newTodo: '',
      todos: [
        { id: '1', title: '吃饭', completed: false },
        { id: '2', title: '睡觉', completed: false },
      ],
    })

    function addTodo() {
      let value = state.newTodo && state.newTodo.trim()
      if (!value) {
        return
      }
      state.todos.push({
        id: state.todos.length + 1,
        title: value,
        completed: false,
      })
      state.newTodo = ''
    }

    const remaining = computed(
      () => state.todos.filter((todo) => !todo.completed).length
    )
    const allDone = computed({
      get: function() {
        return remaining.value === 0
      },
      set: function(value) {
        state.todos.forEach((todo) => {
          todo.completed = value
        })
      }
    })

    function removeCompleted() {
      state.todos = state.todos.filter(todo => !todo.completed)
    }
    return {
      ...toRefs(state),
      addTodo,
      allDone,
      remaining,
      removeCompleted
    }
  },
}
</script>
