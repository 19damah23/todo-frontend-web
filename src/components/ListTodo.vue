<template>
  <div class="listTodo">
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        <button class="btn-todo" @click="editTodo(todo.id)">
          <label>{{ todo.title }}</label>
          <span>{{ todo.description }}</span>
        </button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "ListTodo",
  data: function () {
    return {
      todos: [],
    };
  },
  methods: {
    setTodos(data) {
      this.todos = data;
    },
    editTodo(id) {
      let dataForm = this.todos.find((todo) => todo.id === id);

      this.$root.$emit("emitForm", dataForm);
    },
  },
  mounted() {
    axios
      .get("http://127.0.0.1:8000/api/todos")
      .then((response) => this.setTodos(response.data))
      .catch((error) => console.log(error));
  },
};
</script>

<style>
ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.btn-todo {
  text-align: left;
  border: none;
  border-bottom: 1px solid gainsboro;
  padding: 0 15px;
  cursor: pointer;
  outline: none;
  background: #f7f7f7;
  width: 100%;
  height: 150px;
}

.btn-todo:hover {
  background: #eaeaea;
}

.btn-todo label {
  display: block;
  font-size: 1.5em;
  margin-bottom: 15px;
  color: #444444;
}

.btn-todo span {
  color: #545454;
}
</style>