<template>
  <form
    action=""
    @submit.prevent="onSubmit"
  >
    <input
      type="text"
      name="title"
      id="title"
      v-model="title"
    >

    <button type="submit">
      Create
    </button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      title: '',
    };
  },

  methods: {
    onSubmit() {
      if (this.title.trim()) {
        const newTodo = {
          id: Date.now(),
          title: this.title,
          completed: false,
        };

        this.$emit('add-todo', newTodo);
      }

      this.title = '';
    },
  },
};
</script>

<style scoped>
  form {
    display: flex;
    margin: 0 100px 20px;
  }

  input {
    flex: 1 1 auto;
  }
</style>
