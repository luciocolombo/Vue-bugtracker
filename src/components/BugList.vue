<template>
  <div class="container border shadow maincont">
    <h5 class="info">
      Don't be lazy. Add your Todos here and never skip tasks again
    </h5>

    <input
      type="text"
      placeholder="What need to be done"
      v-model="newTodo"
      @keyup.enter="addTodo"
    />
    <div class="mt-5">
      <div
        class="d-flex justify-content-center mt-2"
        v-for="(todo, index) in todosFiltered"
        :key="todo.id"
      >
        <div class="todo-item-left">
          <input type="checkbox" v-model="todo.completed" />
          <div
            v-if="!todo.editing"
            @dblclick="editTodo(todo)"
            class="todo-item-label listelement"
            :class="{ completed: todo.completed }"
          >
            {{ todo.title }}
          </div>
          <input
            v-else
            class="todo-item-edit"
            type="text"
            v-model="todo.title"
            @blur="stopEdit(todo)"
            @keyup.enter="stopEdit(todo)"
            @keyup.escape="cancelEdit(todo)"
            v-focus
          />
        </div>
        <div class="btn btn-danger boton" @click="removeTodo(index)">
          &times;
        </div>
      </div>
      <div>
        <div class="extra-container">
          <label class="checkboxall"
            ><input
              type="checkbox"
              :checked="!anyRemaining"
              @change="chkboxClick()"
            />{{ checkOrUncheck }}</label
          >
          <div>{{ remaining }} items left</div>
        </div>
        <div class="extra-container">
          <button
            class="w-100"
            :class="{ active: filter == 'all' }"
            @click="filter = 'all'"
          >
            All
          </button>
          <button
            class="w-100"
            :class="{ active: filter == 'active' }"
            @click="filter = 'active'"
          >
            Active
          </button>
          <button
            class="w-100"
            :class="{ active: filter == 'completed' }"
            @click="filter = 'completed'"
          >
            Completed
          </button>
        </div>
        <div class="mt-2">
          <button v-if="showClearCompletedButton" @click="clearCompleted">
            Clear completed
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/* import axios from "axios"; */
export default {
  name: "BugList",
  data() {
    return {
      newTodo: "",
      idForTodo: 3,
      beforeEditCache: "",
      filter: "all",
      checkOrUncheck: "Check All",
      todos: [
        {
          id: 1,
          title: "Finish vue screencsat",
          completed: false,
          editing: false,
        },
        {
          id: 2,
          title: "take over hte world",
          completed: false,
          editing: false,
        },
      ],
    };
  },
  directives: {
    focus: {
      inserted: (el) => {
        el.focus();
      },
    },
  },
  computed: {
    remaining() {
      return this.todos.filter((todo) => !todo.completed).length;
    },
    anyRemaining() {
      return this.remaining != 0;
    },
    todosFiltered() {
      if (this.filter == "all") {
        return this.todos;
      } else if (this.filter == "completed") {
        return this.todos.filter((todo) => todo.completed == true);
      } else if (this.filter == "active") {
        return this.todos.filter((todo) => todo.completed == false);
      }
      return this.todos;
    },
    showClearCompletedButton() {
      return this.todos.filter((todo) => todo.completed == true).length > 0;
    },
  },
  methods: {
    addTodo() {
      if (!this.newTodo.trim() == 0) {
        this.todos.push({
          id: this.idForTodo,
          title: this.newTodo.trim(),
          completed: false,
        }),
          (this.newTodo = "");
        this.idForTodo++;
      }
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    editTodo(todo) {
      this.beforeEditCache = todo.title;

      todo.editing = true;
    },
    stopEdit(todo) {
      if (todo.title.trim() == "") {
        todo.title = this.beforeEditCache;
      }
      todo.editing = false;
    },
    cancelEdit(todo) {
      todo.editing = false;
      todo.title = this.beforeEditCache;
    },
    chkboxClick() {
      this.todos.forEach((todo) => (todo.completed = event.target.checked));
      this.checkOrUncheck === "Check All"
        ? (this.checkOrUncheck = "Uncheck All")
        : (this.checkOrUncheck = "Check All");
    },
    clearCompleted() {
      this.todos = this.todos.filter((todo) => todo.completed == false);
    },
  },
};
</script>

<style scoped>
.todo-item-left {
  display: flex;
  align-items: center;
  min-width: 300px;
}
.todo-item-label {
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}
.todo-item-edit {
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
}
.completed {
  text-decoration: line-through;
  color: grey;
}
.extra-container {
  display: flex;
  justify-content: space-around;
  align-items: center;

  font-size: 16px;
  /*   border-top: 1px solid lightgrey; */
  padding-top: 14px;
  margin-bottom: 14px;
  max-width: 300px;
  margin: auto;
}
.listelement {
  width: 100%;
}
.active {
  background-color: chartreuse;
  font-weight: bold;
}
.maincont {
  background: rgb(211, 211, 211);
  padding: 30px;
}
.boton {
  padding-top: 10px;
}
h5 {
  color: #2c3e50;
  margin-bottom: 3%;
  text-shadow: 3px 4px 5px rgb(109, 109, 109);
}
</style>