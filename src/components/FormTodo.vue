<template>
  <div class="formTodo">
    <form>
      <div class="menu">
        <button
          type="button"
          @click="submitRemove"
          class="btn btn-danger btn-delete"
        >
          Delete
        </button>
        <button
          type="button"
          @click="submitSave"
          v-if="mode == 'save'"
          class="btn btn-primary"
        >
          Save
        </button>
        <button
          type="button"
          @click="submitUpdate"
          v-if="mode == 'update'"
          class="btn btn-primary"
        >
          Update
        </button>
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
  data: function () {
    return {
      id: 0,
      title: "",
      description: "",
      mode: "save",
    };
  },
  methods: {
    submitSave() {
      let params = new URLSearchParams();
      params.append("title", this.title);
      params.append("description", this.description);

      axios.post("http://127.0.0.1:8000/api/todos", params).then((response) => {
        let data = {
          id: response.data.id,
          title: this.title,
          description: this.description,
        };
        this.$root.$emit("emitSaveTodo", data);
      });
    },
    submitUpdate() {
      let params = new URLSearchParams();
      params.append("id", this.id);
      params.append("title", this.title);
      params.append("description", this.description);

      axios
        .put(`http://127.0.0.1:8000/api/todos/${this.id}`, params)
        .then((response) => {
          let data = {
            id: response.data.id,
            title: this.title,
            description: this.description,
          };
          this.$root.$emit("emitUpdateTodo", data);
        });
    },
    submitRemove() {
      let params = new URLSearchParams();
      params.append("id", this.id);

      axios
        .delete(`http://127.0.0.1:8000/api/todos/${this.id}`, params)
        .then((response) => {
          let data = {
            id: response.data.id,
          };
          this.$root.$emit("emitRemoveTodo", data);
          this.resetInput();
        });
    },
    resetInput() {
      this.id = 0;
      this.title = "";
      this.description = "";
    },
  },
  mounted() {
    this.$root.$on("emitForm", (data) => {
      this.id = data.id;
      this.title = data.title;
      this.description = data.description;
      this.mode = data.mode;
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