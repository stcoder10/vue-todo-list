  <template>
  <div id="container">
    <div class="shadow rounded-corners padding-20 margin-15 white-bg overflow-hidden">
      <h1>To-do List</h1>
      <p>This is a simple To-do List application to help you organise your day. Enjoy! </p>
      <form @submit.prevent="addItem">
        <div class="form-div">
          <label for="todo" class="block" >To-Do Item: </label>
          <input class="rounded-corners height-30 block" type="text" id="todo" placeholder="To-Do Item" v-model="newTodo">
        </div>
        <div class="form-div">
          <label for="date" class="block">Date: </label>
          <input class="height-30 rounded-corners block" type="date" id="date" v-model="newDate"><br>
        </div>
        <div class="form-div">
          <input class="white-bg button height-30 block" type="submit" value="Submit">
        </div>
      </form>
    </div>

    <ul v-if="hasTodos">
      <li class="shadow rounded-corners padding-20 block margin-15 white-bg" v-for="todo in todos">
        <input type="checkbox" v-model="todo.done" :id="todo.id"><label :for="todo.id">{{ todo.text }} - {{ todo.date
        }}</label>
        <button class="button delete-button padding-5 white-bg" @click="deleteItem(todo)" :key="todo.id">X</button>
      </li>
    </ul>
    <div v-else class="shadow rounded-corners padding-20 block margin-15 white-bg">Great job! You're all done!</div>
  </div>
</template>

<script>
import HeaderComp from './components/HeaderComp.vue'
class Todo {
  constructor(id, text) {
    this.id = id
    this.text = text
  }
}

export default {
  data() {
    return {
      newTodo: "",
      newDate: "No Date",
      dateToUse: "",
      todos: [],
      hasTodos: true,
      uName: "",
      sampleTodo: { id: 0, text: "No Tasks", done: false },
      id: 0,
    };
  },
  methods: {
    addItem() {
      if (this.newDate === "") {
        this.dateToUse = "No Date"
      }
      else {
        this.dateToUse = this.newDate
      }
      this.id++
      if (this.todos[0] === this.sampleTodo) {
        this.todos[0] = { id: this.id, text: this.newTodo, date: this.dateToUse, done: false }
      }
      else {
        this.todos.push({ id: this.id, text: this.newTodo, date: this.dateToUse, done: false })
      }
      this.newTodo = ""
      this.newDate = "No Date"
      this.hasTodos = true
    },
    deleteItem(todo) {
      this.todos = this.todos.filter((t) => t !== todo);
      if (this.todos.length === 0) {
        this.hasTodos = false
      }
    },
    updateTodos() {
      localStorage.setItem("todos", JSON.stringify(this.todos))
    },
  },
  mounted() {
    if (JSON.parse(localStorage.getItem("todos"))) {
      this.todos = JSON.parse(localStorage.getItem("todos"))
    }
    else {
      this.todos[0] = this.sampleTodo
    }
    if (localStorage.getItem("maxID")) {
      this.id = localStorage.getItem("maxID")
    }
    else {
      this.id = 0
    }
  },
  updated() {
    this.updateTodos()
  },
  watch: {
    id(newID) {
      localStorage.setItem("maxID", newID)
    }
  },
}
</script>
<style>
* {
  box-sizing: border-box;
}

body {
  background-color: #f0f0f0;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

.white-bg {
  background-color: white;
}

.shadow {
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
}

.block {
  display: block;
}

.rounded-corners {
  border-radius: 5px;
}

.padding-20 {
  padding: 20px;
}

.margin-15 {
  margin: 15px;
}

.date {
  float: right;
}

.form-div {
  display: block;
  margin-top: 15px;
}

.button {
  border-radius: 5px;
  height: 30px;
  border-color: lightgrey;
  border-style: solid;
  transition: 0.1 ease-in-out;
}

.delete-button {
  width: 40px;
  float: right;
}


.button:hover {
  background-color: lightgray;
}

.overflow-hidden {
  overflow: hidden;
}

.height-30 {
  height: 30px;
}

ul {
  margin: 0;
  padding: 0;
  width: 100%;
}

#container {
  max-width: 700px;
  width: 100%;
  margin: auto;
}
input {
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

input[type="submit"], input[type="text"], input[type="date"] {
  width: 100%;
  float: right;
  border-style: solid;
  border: 2px solid lightgrey;
}
</style>