<template>
  <ul class="list-group">
    <li class="list-group-item" v-for="(todo, index) in todos" :key="index">
      <div class="row">
        <div class="col-8">
          <span v-if="todo.isDone">
            <del>{{ todo.activity }}</del>
          </span>
          <span v-else>{{ todo.activity }}</span>
        </div>
        <div class="col-2">
          <button class="btn btn-info" @click="done(index)"><i class="bi bi-check-circle"></i></button>
        </div>
        <div class="col-2">
          <button class="btn btn-danger" @click="deleteTODO(index)"><i class="bi bi-trash"></i></button>
        </div>
      </div>
    </li>
  </ul>
</template>

<script>
export default {
  props: {
    todos: {
      type: Array,
      default() {
        return []
      }
    }
  },
  setup(props, { emit }) {
    const deleteTODO = index => {
      emit('deleteTodo', index)
    };

    const done = index => {
      emit('doneTodo', index)
    }

    return { deleteTODO, done}
  },
  // methods: {
  //   deleteTODO(index) {
  //     this.$emit('deleteTodo', index)
  //   },
  //   done(index) {
  //     this.$emit('doneTodo', index)
  //   }
  // }
}
</script>
