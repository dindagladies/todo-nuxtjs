<template>
  <header class="header">
    <h1>Super2Do</h1>
    <form @submit="storeData" id="storeForm">
      <input
        v-model="task.name"
        class="new-todo input"
        :placeholder="placeholder"
        autofocus
        ref="name"
      />
      <button type="submit"></button>
    </form>
  </header>
</template>
<script>
export default {
  props: ["placeholder", "changeNeedUpdate"],
  data() {
    return {
      task: { name: "", status: "ACTIVE" },
    };
  },

  watch: {
    "task.name"(newTask) {
    },
  },

  methods: {
    storeData(event) {
      event.preventDefault();
      this.$axios
        .post(`/api/tasks/`, this.task)
        .catch((error) => {
          console.log(error.response.task);
        })
        .finally(() => {
          this.task.name = ""; 
          this.changeNeedUpdate(true);
        });
    },
  },
};
</script>