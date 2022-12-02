<template>
  <footer class="footer">
    <!-- This should be `0 items left` by default -->
    <span class="todo-count"
      ><strong>{{ tasks.length }}</strong> item left</span
    >
    <!-- Remove this if you don't implement routing -->
    <ul class="filters">
      <li>
        <a
          href="#"
          :class="{selected: isSelected == 1}"
          @click="clicked(); makeSelected(1)"
          >All</a
        >
      </li>
      <li>
        <a href="#" :class="{selected: isSelected == 2}" @click="clicked('ACTIVE'); makeSelected(2)">Active</a>
      </li>
      <li>
        <a href="#" :class="{selected: isSelected == 3}" @click="clicked('COMPLETED'); makeSelected(3)">Completed</a>
      </li>
    </ul>
    <!-- Hidden if no completed items are left ↓ -->
    <button class="clear-completed" @click="destroyCompleted">
      Clear completed
    </button>
  </footer>
</template>
<script>
export default {
  props: ["getStatus", "changeNeedUpdate", "tasks"],
  data() {
    return {
      task: {},
      isSelected: 1,
    };
  },

  watch: {
    "task.status"(newStatus) {
      console.log(newStatus);
    },
  },
  methods: {
    clicked(status) {
      console.log(status, "in footer");
      this.getStatus(status);
    },

    destroyCompleted() {
      this.$axios
        .delete(`/api/tasks/delete?status=COMPLETED`)
        .catch((error) => {
          console.log(error.response.task);
        })
        .finally(() => {
          this.changeNeedUpdate(true);
        });
    },

    makeSelected(value) {
      this.isSelected = value;
    }
  },
};
</script>