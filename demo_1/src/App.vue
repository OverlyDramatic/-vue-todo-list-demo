<template>
  <div class="todo-container">
    <div class="todo-wrap">
      <ToDoHeader :addTodo="addTodo"/>
      <ToDoList :todos="todos" :deleteTodo="deleteTodo"/>
      <ToDoFooter :todos="todos" :deleteCompleteTodos="deleteCompleteTodos" :selectAllTodos="selectAllTodos"/>
    </div>
  </div>
</template>
<script>
  import ToDoHeader from './components/ToDoHeader.vue'
  import ToDoList from './components/ToDoList.vue'
  import ToDoFooter from './components/ToDoFooter.vue'

  export default{
    components: {
      ToDoHeader,
      ToDoList,
      ToDoFooter
    },
    data () {
      return {
        // 从local storage中读取todos数据
        todos: JSON.parse(window.localStorage.getItem('todo_key') || '[]')
        // [
        //   {title: '吃饭', complete: false},
        //   {title: '睡觉', complete: false},
        //   {title: '洗澡', complete: false}
        // ]
      }
    },
    methods: {
      addTodo (todo) {
        this.todos.unshift(todo)
      },
      deleteTodo (index) {
        this.todos.splice(index, 1)
      },
      deleteCompleteTodos () {
        this.todos = this.todos.filter(todo => !todo.complete)
      },
      selectAllTodos (check) {
        if (check) {
          this.todos.forEach((todo, index) => {
            todo.complete = true
          })
        } else {
          this.todos.forEach((todo, index) => {
            todo.complete = false
          })
        }
      }
    },
    watch: { // 深度监视
      todos: {
        deep: true, // 深度监视true
        handler: function (newV, oldV) {
          // 将todos最新值保存到storage,json格式
          window.localStorage.setItem('todo_key', JSON.stringify(newV))
        }
      }
    }
  }
</script>
<style>
  /*app*/
  .todo-container {
    width: 500px;
    margin: 0 auto;
  }
  .todo-container .todo-wrap {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }

</style>
