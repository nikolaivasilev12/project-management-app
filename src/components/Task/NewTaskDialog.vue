<template>
  <el-dialog
    :before-close="close"
    title="New Task"
    v-model="taskModalDialog"
    width="30%"
  >
    <el-form>
      <el-form-item label="Task name">
        <el-input
          placeholder="Enter task name"
          v-model="name"
          autocomplete="off"
        ></el-input>
      </el-form-item>
      <el-form-item label="Task desription">
        <el-input
          type="textarea"
          placeholder="Enter task description"
          v-model="description"
          autocomplete="off"
        ></el-input>
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="close()">Cancel</el-button>
        <el-button type="primary" @click="submitTask(dialogData._id)"
          >Add Task</el-button
        >
      </span>
    </template>
  </el-dialog>
</template>

<script>
import axios from "axios";
export default {
  props: ["taskModalDialog", "dialogData"],
  emits: ["create", "close"],
  data: () => ({
    name: "",
    description: "",
    colid: "",
  }),
  methods: {
    close() {
      this.$emit("close");
    },
    async submitTask(colid) {
      await axios
        .put(`http://localhost:3000/api/projects/addtask/${colid}`, {
          task_name: this.name,
          task_description: this.description,
        })
        .then((response) => {
          (this.tasks = response.data), this.close(), this.$emit("create");
        });
    },
  },
};
</script>