<template>
  <div class="todo-footer">
    <label>
      <input type="checkbox" v-model="isAllChecked"/>
    </label>
    <span>
      <span>已完成{{ completeItems }}</span> / 全部 {{ todos.length }}
    </span>
    <button class="btn btn-danger" @click="deleteCompleteTodos" v-show="completeItems">清除已完成任务</button>
  </div>
</template>
<script>
  export default{
    props: {
      todos: Array,
      deleteCompleteTodos: Function,
      selectAllTodos: Function
    },
    data () {
      return {
        // isAllChecked: false
      }
    },
    computed: {
      completeItems () {
        return this.todos.reduce((preTotal, todo) => preTotal + (todo.complete ? 1 : 0), 0)
      },
      isAllChecked: {
        get () {
          return this.completeItems === this.todos.length && this.completeItems > 0
        },
        set (value) { // value 为CheckBox最新值
          this.selectAllTodos(value)
        }
      }
    }
  }
</script>
<style>
  /*footer*/
  .todo-footer {
    height: 40px;
    line-height: 40px;
    padding-left: 6px;
    margin-top: 5px;
  }

  .todo-footer label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
  }

  .todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 5px;
  }

  .todo-footer button {
    float: right;
    margin-top: 5px;
  }

</style>
