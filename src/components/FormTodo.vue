<template>
  <div class="formTodo">
    <form @submit="submitTodo">
      <div class="menu">
        <button type="button" class="btn btn-danger btn-delete">Delete</button>
        <button type="submit" class="btn btn-primary">Save</button>
      </div>

      <div class="content">
        <input type="hidden" v-model="id" />
        <input
          type="text"
          class="text"
          placeholder="Title of todo"
          v-model="title"
        />
        <textarea
          class="text textarea"
          placeholder="Description of todo"
          v-model="description"
        ></textarea>
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "FormTodo",
  props: {
    propSaveNote: {
      type: Function,
    },
  },
  data: function () {
    return {
      title: "",
      description: "",
    };
  },
  methods: {
    submitTodo(e) {
      e.preventDefault();

      axios.post("http://localhost:8000/api/add-todo", {
        title: "" + this.title,
        description: "" + this.description,
      });

      this.title = "";
      this.description = "";
    },
  },
  mounted() {
    this.$root.$on("emitForm", (data) => {
      this.id = data.id;
      this.title = data.title;
      this.description = data.description;
    });
  },
};
</script>

<style>
.menu {
  padding: 10px 25px;
  background: #f7f7f7;
  margin-bottom: 25px;
  text-align: right;
  border-bottom: 1px solid #e8e6e6;
}

.btn-delete {
  margin-right: 10px;
}

.content {
  padding: 0 25px;
}

.text {
  display: block;
  width: 100%;
  padding: 0;
  font-size: 20px;
  font-weight: bold;
  color: #2c3e5e;
  border: none;
  margin-bottom: 10px;
  box-sizing: border-box;
  outline: none;
}

.textarea {
  min-height: 350px;
  font-size: 15px;
  font-weight: lighter;
  line-height: 30px;
}

.loader {
  vertical-align: middle;
}
</style>