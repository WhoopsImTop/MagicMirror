<template>
  <div class="todo-container">
      <h1>Todo's</h1>
      <div v-for="todo in todos" :key="todo._id" class="todoItem">
          <input @click="!todo.checked" :checked=todo.checked type="checkbox">
          <span v-html="todo.titel" :class="todo.checked ? 'none' : 'linethrough'" class="titel"></span>
      </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
    name: "Todo",
    data() {
        return {
            todos: []
        }
    },
    methods: {
        fetchTodos() {
            axios
            .get('http://192.168.178.81:3004/api/todos')
            .then(Response => {
                this.todos = Response.data
            })
            .catch((e) => {
                console.info(e)
                this.todos = [{
                    "_id": 204125,
                    "titel": "Keine Verbindung zum Server möglich",
                    "checked": false
                }]
            })
        }
    },
    created() {
        this.fetchTodos()
    }
}
</script>

<style>
.todo-container {
    padding: 10px;
    color: #efefef;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
}

.todoItem {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
}

input {
    width: 20px;
    height: 20px;
    border: 1px solid #efefef;
    border-radius: 2px;
    margin-right: 15px;
}

</style>