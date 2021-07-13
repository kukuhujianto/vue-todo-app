<template>
  <div class="container" style="margin-top: 20px;">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">Simple Todo App</h5>
        <div class="row">
          <div class="col-10">
            <!-- @keyup.enter="add" >> jika klik enter akan menjalankan method add -->
            <input
              v-model="todo"
              type="text"
              class="form-control"
              @keyup.enter="add"
            />
          </div>
          <div class="col-2">
            <button class="btn btn-success" @click="add">Add</button>
          </div>
        </div>
        <br />
        <list :todos="list" @deleteTodo="deleteTodo" @doneTodo="doneTodo" />
        <small>Total Todo: {{ totalTODO }}</small>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive, toRefs, onMounted, computed } from "vue";
import List from "./components/List.vue";

export default {
  components: { List },
  setup() {
    const todo = ref("");
    const todos = reactive({
      list: []
    });

    //onMounted = fungsi yang pertama kali dijalankan
    onMounted(() => {
      const items = localStorage.getItem("todos");
      //if isi items ada maka ? simpan di local storage, jika tidak ada maka : array kosong
      todos.list = items ? JSON.parse(items) : [];
    });

    const totalTODO = computed(() => {
      return todos.list.length;
    });

    const add = () => {
      todos.list.unshift({
        activity: todo.value,
        isDone: false
      });
      todo.value = "";
      saveToLocalStorage();
    };

    const deleteTodo = todoIndex => {
      todos.list = todos.list.filter((item, index) => {
        if (index != todoIndex) {
          return item;
        }
      });
      saveToLocalStorage();
    };

    const doneTodo = todoIndex => {
      todos.list = todos.list.filter((item, index) => {
        if (index == todoIndex) {
          item.isDone = true;
        }
        return item;
      });
      saveToLocalStorage();
    };

    const saveToLocalStorage = () => {
      localStorage.setItem("todos", JSON.stringify(todos.list));
    };

    return { todo, ...toRefs(todos), totalTODO, add, deleteTodo, doneTodo };
  }
};
</script>
