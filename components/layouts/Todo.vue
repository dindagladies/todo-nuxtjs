<template>
  <section class="main">
    <input id="toggle-all" class="toggle-all" type="checkbox" />
    <label for="toggle-all">Mark all as complete</label>
    <ul class="todo-list">
      <li
        v-for="task in tasks"
        :key="task.id"
        :class="task.status === 'COMPLETED' ? 'completed' : ''"
      >
        <div class="view">
          <input
            @input="
              changeStatus(
                task.id,
                $event.target.checked ? 'COMPLETED' : 'ACTIVE'
              )
            "
            class="toggle"
            type="checkbox"
            :checked="task.status === 'COMPLETED'"
          />
          <label>{{ task.name }}</label>
          <button @click="destroy(task.id)" class="destroy"></button>
        </div>
        <input class="edit" value="Create a Todo template" />
      </li>
    </ul>
  </section>
</template>
<script>
export default {
  props: ["tasks", "getTasks", "changeStatus", "changeNeedUpdate"],

  methods: {
    destroy(id) {
      this.$axios
        .delete(`/api/tasks/delete?id=${id}`)
        .catch((error) => {
          console.log(error.response.task);
        })
        .finally(() => {
          this.changeNeedUpdate(true);
        });
    },
  },
};
</script>