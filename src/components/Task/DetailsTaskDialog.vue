<template>
  <el-dialog
    :before-close="close"
    title="DETAILS"
    v-model="detailsTaskDialog"
    width="50%"
  >
    <el-main>
      <el-row type="flex">
        <el-col :span="7" :offset="1">
          <h1>Task Details</h1>
          <p class="bold mt-2">Task name</p>
          <el-form>
            <el-form-item>
              <el-input v-model="editName" autocomplete="off"></el-input>
            </el-form-item>
            <p class="bold mt-2">Task Description</p>
            <el-form-item>
              <el-input
                type="textarea"
                v-model="editDescription"
                autocomplete="off"
              ></el-input>
            </el-form-item>
          </el-form>
        </el-col>
        <el-col :span="6" :offset="1">
          <h1>Priority</h1>
          <p class="bold mt-2">Priority of the task</p>
          <el-select v-model="value" placeholder="Choose Priority">
            <el-option
              v-for="priority in priorities"
              :key="priority.value"
              :label="priority.label"
              :value="priority.value"
            >
            </el-option>
          </el-select>
        </el-col>
        <el-col :span="8" :offset="1">
          <h1>Actions</h1>
          <p class="bold mt-2">Assign members to task</p>
          <AddMembers />
          <br /><br />
          <p class="bold mt-2">Task State</p>
          <el-radio-group v-model="taskProgress" size="small">
            <el-radio-button label="In Progress"></el-radio-button>
            <el-radio-button label="On Hold"></el-radio-button>
            <el-radio-button label="Completed"></el-radio-button>
          </el-radio-group>

          <p class="bold mt-3">Estimate time to complete</p>
          <el-input-number
            v-model="timeSelect"
            :precision="2"
            :step="0.1"
            :max="24"
            :min="0"
          ></el-input-number>
          <el-button
            id="mt-3"
            class="mt-3"
            icon="el-icon-delete"
            @click="removeTask(detailsTaskDialogData._id)"
            >Delete Task</el-button
          >
        </el-col>
      </el-row>
    </el-main>

    <template #footer>
      <span class="dialog-footer">
        <el-button @click="close()">Cancel</el-button>
        <el-button type="primary" @click="submitTask(detailsTaskDialogData._id)"
          >Submit</el-button
        >
      </span>
    </template>
  </el-dialog>
</template>

<script>
import AddMembers from "./AddMembers.vue";
import axios from "axios";
export default {
  components: {
    AddMembers,
  },
  props: ["detailsTaskDialog", "detailsTaskDialogData"],
  emits: ["submit", "close"],
  data: () => ({
    editName: "",
    editDescription: "",
    taskId: "",

    priorities: [
      {
        value: "Option1",
        label: "Low",
      },
      {
        value: "Option2",
        label: "Medium",
      },
      {
        value: "Option3",
        label: "High",
      },
      {
        value: "Option4",
        label: "Very High",
      },
    ],
    taskProgress: "In Progress",
    value: "",
    timeSelect: 0,
  }),
  mounted() {
    this.editName = this.detailsTaskDialogData.task_name;
    this.editDescription = this.detailsTaskDialogData.task_description;
    this.taskId = this.detailsTaskDialogData._id;
  },
  methods: {
    async submitTask(taskId) {
      await axios
        .put(`http://localhost:3000/api/projects/updatetask/${taskId}`, {
          task_name: this.editName,
          task_description: this.editDescription,
        })
        .then((response) => {
          (this.tasks = response.data), this.close(), this.$emit("submit");
        });
    },

    async removeTask(taskid) {
      await axios
        .put(`http://localhost:3000/api/projects/deletetask/${taskid}`)
        .then((response) => {
          (this.tasks = response.data), this.close(), this.$emit("submit");
        });
    },

    close() {
      this.$emit("close");
    },
  },
};
</script>

<style scoped>
#mt-3 {
  margin-top: 30px;
}
.bold {
  font-weight: bold;
}
.mt-2 {
  margin-top: 10px;
}
</style>