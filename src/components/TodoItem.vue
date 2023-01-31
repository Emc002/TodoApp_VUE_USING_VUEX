<template>
  <div class="todo-item" @dblclick="editTodo">
    <div class="todo-item-left" >
      <input type="checkbox" class="checkbox" v-model="completed" @change="doneEdit">
      <div v-if="!editing" :class="{ completed:completed }"
        class="todo-item-label">{{ title }}</div>
      <div v-else class="edit-item" >
      <input type="text" class="todo-item-edit"  v-model="title"
        @blur="doneEdit" @keyup.esc="cancelEdit" @keyup.enter="doneEdit" v-focus>
        <label class="editing-label">Editing</label>
      </div>

    </div>
    <div class="remove-item" @click="removeTodo(index)">
      &times;
    </div>

  </div>
</template>

<script>
const focus = {
  mounted: (el) => el.focus()
}

export default {
  name: 'todo-item',
  props:  {
    todo: {
      type: Object,
      required: true,
    },
    checkAll: {
      type: Boolean,
      required: true,
    }
  },

  data() {
    return {
      'id':this.todo.id,
      'title':this.todo.title,
      'completed':this.todo.completed,
      'editing':this.todo.editing,
      'beforeEditCache': ''
    }
  },
  directives: {
    focus
  },
  watch: {
    checkAll(){
      this.completed = this.checkAlls ? true : this.todo.completed
    }
  },

  methods: {
    removeTodo(id) {
      this.$store.dispatch('deleteTodo', id)
    },
    editTodo(){
      this.beforeEditCache = this.title
      this.editing = true
    },
    cancelEditTodo() {
      this.title = this.beforeEditCache
      this.editing = false
    },

    doneEdit() {
      if (this.title.trim() == '') {
        this.title = this.beforeEditCache
      }
      this.editing = false
      this.$store.dispatch('updateTodo', {
        'id': this.id,
        'title': this.title,
        'completed': this.completed,
        'editing': this.editing,
      })
    },
    cancelEdit() {  
      this.title = this.beforeEditCache
      this.editing = false
    },
}
}
</script>

<style lang="scss">
.todo-item-left .checkbox{
  padding: 1rem !important;
}

.todo-item-edit{
  align-items: center;
  justify-content: space-between;
  animation-duration: 0.3s;
  padding: 12px;
  border-radius: 12px;
  border: 1.5px solid lightgrey;
  outline: none;
  transition: all 0.3s cubic-bezier(0.19, 1, 0.22, 1);
  box-shadow: 0px 0px 20px -18px;
  transform: scale(0.95);

  &:focus{
    border: 2px solid lightgreen;
    color: lightgreen;
  }
}
</style>