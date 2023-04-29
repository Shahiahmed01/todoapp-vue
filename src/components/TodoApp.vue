<template>
  <div class="container w-50">
    <h2 class="text-center mt-5">Todo App</h2>

    <!-- Task table -->
    <div class="p-4" style="border: 1px black solid">
      <div class="d-flex justify-content-center">
        <input v-model="task" type="text" placeholder="Add task" />
        <button @click="submitTask" class="btn btn-warning rounded-0">
          Submit
        </button>
      </div>
      <table class="table">
        <thead>
          <tr>
            <th scope="col ">Task</th>
            <th scope="col ">Status</th>
            <th scope="col">Edit</th>
            <th scope="col">Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in tasks" :key="index">
            <td :class="{ finished: task.status === 'finished' }">
              <span>{{ task.name }}</span>
            </td>
            <td style="width: 120px">
              <span
                @click="changeStatus(index)"
                class="pointer"
                :class="{
                  'text-danger': task.status === 'to-do',
                  'text-warning': task.status === 'in-progress',
                  'text-success': task.status === 'finished',
                }"
                >{{ firstCharUpper(task.status) }}</span
              >
            </td>
            <td>
              <div @click="editTask(index)">
                <span><i class="ph-light ph-pencil"></i></span>
              </div>
            </td>
            <td>
              <div @click="deleteTask(index)">
                <i class="ph-light ph-trash"></i>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      availableStatuses: ["to-do", "in-progress", "finished"],
      editedTask: null,
      task: "",
      tasks: [
        {
          name: "Make todo app",
          status: "to-do",
        },
        {
          name: "Play footbal with friends",
          status: "in-progress",
        },
        {
          name: "Do homework",
          status: "to-do",
        },
      ],
    };
  },
  methods: {
    submitTask() {
      if (this.task.length === 0) return;
      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "to-do",
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
      this.task = "";
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);

      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
  },
};
</script>
<style>
.pointer {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
}
</style>
