<template>
  <div>
    <LayoutsHeader
      placeholder="What needs to be done?"
      :changeNeedUpdate="changeNeedUpdate"
    />
    <LayoutsTodo
      :tasks="tasks"
      :getTasks="getTasks"
      :changeStatus="changeStatus"
      :changeNeedUpdate="changeNeedUpdate"
    />
    <LayoutsFooter
      :tasks="tasks"
      :getStatus="getStatus"
      :changeNeedUpdate="changeNeedUpdate"
    />
    <Nuxt />
  </div>
</template>
<script>
export default {
  data() {
    return {
      fields: ["id", "name", "status"],
      tasks: [],
      needUpdate: false,
      status: "",
    };
  },

  watch: {
    needUpdate(isNeedUpdate) {
      if (!isNeedUpdate) {
        return;
      }

      this.getTasks(this.status).finally(() => {
        this.needUpdate = false;
      });
    },
    status(isStatusChange) {
      this.getTasks(this.status).finally(() => {
        this.status = this.status;
      });
    },
  },

  mounted() {
    this.getTasks(this.status);
  },

  methods: {
    async getTasks(status) {
      console.log(status, "in getTasks()");
      let url =
        typeof status == "undefined"
          ? `/api/tasks`
          : `/api/tasks?status=${status}`;
      return this.$axios
        .get(url)
        .then((response) => {
          this.tasks = response.data.data;
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    },

    changeStatus(id, status) {
      this.$axios
        .put(`/api/tasks/${id}`, { status })
        .catch((error) => {
          console.log(error.response.data);
        })
        .finally(() => {
          console.log(this.needUpdate, "change status");
          this.needUpdate = true;
        });
    },

    changeNeedUpdate(value) {
      this.needUpdate = value;
    },

    getStatus(value) {
      this.status = value;
      console.log(this.status, "in getStatus()");
    },
  },
};
</script>
<style>
</style>