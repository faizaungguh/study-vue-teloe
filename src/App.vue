<template>
  <div class="container" style="margin-top: 20px">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">SIMPLE TODO APP</h5>
        <div class="row">
          <div class="col-10">
            <input
              v-model="todo"
              type="text"
              class="form-control"
              @keyup.enter="add"
            />
          </div>
          <div class="col-2">
            <button class="btn btn-success" @click="add">ADD</button>
          </div>
        </div>
        <list :todos="todos" @deleteTodo="deleteTodo" @doneTodo="doneTodo" />
        <br />
        <small>Total TODO : {{ totalTODO }}</small>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive, unMounted } from "vue";
import List from "./components/List.vue";
export default {
  components: { List },
  setup() {
    const todo = ref('');
    const todos = reactive([]);

    onMounted(()=>{
      todos.push({a: 'test'})
      console.log
    })
  },
  data() {
    return {
      todo: "",
      todos: [],
    };
  },
  mounted() {
    this.todos = JSON.parse(localStorage.getItem("todos"));
  },
  computed: {
    totalTODO() {
      return this.todos.length;
    },
  },
  methods: {
    add() {
      this.todos.unshift({
        activity: this.todo,
        isDone: false,
      });
      this.todo = "";
      this.saveLocalStorage();
    },
    deleteTodo(todoIndex) {
      this.todos = this.todos.filter((item, index) => {
        if (index != todoIndex) {
          return item;
        }
      });
      this.saveLocalStorage();
    },
    doneTodo(todoIndex) {
      this.todos = this.todos.filter((item, index) => {
        if (index == todoIndex) {
          item.isDone = true;
        }
        return item;
      });
      this.saveLocalStorage();
    },
    saveLocalStorage() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
  },
};
</script>
