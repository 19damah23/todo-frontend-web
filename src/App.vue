<template>
  <div id="app">
    <div class="left">
      <div class="logo">
        <a href="">
          <H2>Sulkus<span>todo</span> </H2>
        </a>
      </div>
      <div class="frame-todo">
        <button @click="newTodo" class="btn btn-primary btn-add-todo">
          + Add Todo
        </button>

        <ListTodo />
      </div>
    </div>
    <div class="right">
      <FormTodo />
    </div>
  </div>
</template>

<script>
import ListTodo from "@/components/ListTodo.vue";
import FormTodo from "@/components/FormTodo.vue";

export default {
  name: "App",
  data: function () {
    return {};
  },
  components: {
    ListTodo,
    FormTodo,
  },
  methods: {
    newTodo() {
      let dataForm = {
        id: 0,
        title: "",
        description: "",
        mode: "save"
      };

      this.$root.$emit("emitForm", dataForm);
    },
  },
};
</script>

<style>
body {
  margin: 0;
  overflow: hidden;
}

#app {
  font-family: Arial, Helvetica, sans-serif;
  padding: 0;
  display: flex;
  color: #2e3c50;
  widows: 100%;
}

.left {
  width: 400px;
  color: #616161;
  background: #f7f7f7;
}

.logo {
  padding: 25px 15px;
  border-bottom: 1px solit gainsboro;
}

.logo a {
  text-decoration: none;
}

.logo a h2 {
  margin: 0;
  display: inline;
  margin-right: 5px;
  font-size: 35px;
  text-transform: capitalize;
  color: #757575;
}

.logo a h2 span {
  font-size: 12px;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: #0057ff;
}

.frame-todo {
  overflow-y: scroll;
  overflow-x: hidden;
  height: 85vh;
}

.btn-danger {
  background: #ff0000;
}

.btn-danger:hover {
  background: #c50000;
}

.btn-primary {
  background: #0057ff;
}

.btn-primary:hover {
  background: #0000ff;
}

.btn {
  font-size: 12px;
  border: none;
  text-align: center;
  letter-spacing: 1px;
  cursor: pointer;
  border-radius: 2px;
  padding: 7px 25px;
  outline: none;
}

.btn-add-todo {
  width: 90%;
  padding: 12px 10px;
  margin: 10px 15px;
  text-align: left !important;
  color: #fff;
}

.right {
  width: 100%;
  overflow-y: scroll;
  height: 100vh;
  border-left: 1px solid gainsboro;
}

::-webkit-scrollbar {
  width: 5px;
}

::-webkit-scrollbar-track {
  color: #fff;
}

::-webkit-scrollbar-thumb {
  background: #888;
}

::-webkit-scrollbar-thumb:hover {
  background: #555;
}
</style>
