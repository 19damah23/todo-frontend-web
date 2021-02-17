<template>
  <div class="listTodo">
    <ul>
      <li v-for="(row, index) in todos" :key="index">
        <button @click="editTodo(row.id)" class="btn-todo">
          <label>{{ row.title }}</label>
          <span>{{ row.description }}</span>
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
    editTodo(id) {
      let dataForm = this.todos.find((todo) => todo.id === id);
      dataForm.mode = "update";

      this.$root.$emit("emitForm", dataForm);
    },
    getData() {
      axios.get("http://127.0.0.1:8000/api/todos").then((response) => {
        this.todos = response.data;
      });
    },
  },
  mounted() {
    this.getData();

    this.$root.$on("emitRemoveTodo", (data) => {
      let todoIndex = this.todos.findIndex((todo) => todo.id === data.id);
      this.todos.splice(todoIndex, 1);
    });

    this.$root.$on("emitUpdateTodo", (data) => {
      let todoIndex = this.todos.findIndex((todo) => todo.id === data.id);

      this.todos[todoIndex].title = data.title;
      this.todos[todoIndex].description = data.description;
    });

    this.$root.$on("emitSaveTodo", (data) => {
      let newTodo = {
        id: data.id,
        title: data.title,
        description: data.description,
      };

      this.todos.unshift(newTodo);
      this.editTodo(data.id);
    });
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