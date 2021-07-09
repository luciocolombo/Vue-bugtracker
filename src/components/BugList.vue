<template>
  <div class="container border shadow maincont mainbg">
    <h5 class="info">
      <i class="fas fa-thumbtack"></i> Don't be lazy. Add your Todos here and
      never skip tasks again
    </h5>

    <i class="fas fa-tasks font"></i>
    <input
      type="text"
      placeholder="What need to be done"
      v-model="newTodo"
      @keyup.enter="addTodo"
    />

    <div class="mt-5 taskdiv">
      <div class="extradiv">
        <div
          class="d-flex mt-2 justify-content-center"
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
          <button class="boton" @click="removeTodo(index)">
            <i class="fas fa-times"></i>
          </button>
        </div>
      </div>
      <div>
        <div class="extra-container mt-5">
          <label class="checkboxall"
            ><input
              type="checkbox"
              :checked="!anyRemaining"
              @change="chkboxClick()"
            />{{ checkOrUncheck }}</label
          >
          <div>
            <strong>{{ remaining }}</strong> items left
          </div>
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
          <transition name="fade">
            <button v-if="showClearCompletedButton" @click="clearCompleted">
              Clear completed
            </button>
          </transition>
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
          title: "Be the best Fullstack Developer of the World",
          completed: false,
          editing: false,
        },
        {
          id: 2,
          title: "Visit luciocolombodev.netlify.app now",
          completed: false,
          editing: false,
        },
        {
          id: 3,
          title: "Be the best Web Developer",
          completed: false,
          editing: false,
        },
        {
          id: 4,
          title: "Learn ReactJS, MongoDB, ExpressJS and NodeJS",
          completed: true,
          editing: false,
        },
        {
          id: 5,
          title:
            "Help the community with a free project (www.mascotasperdidas.club)",
          completed: true,
          editing: false,
        },
        {
          id: 6,
          title: "Read again Refactoring UI by Steve Schongen",
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
  width: 600px;
  background-color: cornsilk;
  border: 1px solid black;
  box-shadow: 4px 4px 3px teal;
}
.active {
  background-color: rgba(0, 255, 85, 0.644);
  font-weight: bold;
}
.maincont {
  background: rgb(211, 211, 211);
  padding: 30px;
}

h5 {
  color: #2c3e50;
  margin-bottom: 3%;
  text-shadow: 3px 4px 5px rgb(109, 109, 109);
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.6s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
.boton {
  background-color: rgba(255, 0, 0, 0.8);
  min-width: 30px;
  margin-left: 10px;
}

.mainbg {
  background: #0098f0;
  background: -webkit-gradient(
    linear,
    left bottom,
    left top,
    from(#0098f0),
    to(#00f2c3)
  );
  background: linear-gradient(0deg, #0098f0, #00f2c3);
}
.font {
  margin-right: 10px;
}
</style>