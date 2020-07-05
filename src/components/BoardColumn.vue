<template>
  <AppDrop @drop="moveTaskOrColumn">
    <AppDrag
      class="column"
      :transferData="{
      type: 'column',
      fromColumnIndex: columnIndex
  }"
    >
      <div class="flex items-center mb-2 font-bold">{{ column.name }}</div>
      <!--TASK-->
      <div class="list-reset">
        <column-task
          v-for="(task, $taskIndex) of column.tasks"
          :key="$taskIndex"
          :task="task"
          :taskIndex="$taskIndex"
          :columnIndex="columnIndex"
          :column="column"
          :board="board"
        ></column-task>

        <input
          type="text"
          class="block px-2 w-full bg-transparent outline-none"
          placeholder="+ Enter new task"
          @keyup.enter="createTask($event, column.tasks)"
        />
      </div>
    </AppDrag>
  </AppDrop>
</template>

<script>
import ColumnTask from "../components/ColumnTask.vue";
import AppDrag from "../components/AppDrag.vue";
import AppDrop from "../components/AppDrop.vue";
import movingTasksAndColumnsMixin from "../mixins/movingTasksAndColumnsMixin.js";
export default {
  mixins: [movingTasksAndColumnsMixin],
  components: {
    ColumnTask,
    AppDrag,
    AppDrop
  },
  methods: {
    pickupColumn(e, fromColumnIndex) {
      e.dataTransfer.effectAllowed = "move";
      e.dataTransfer.dropEffect = "move";

      e.dataTransfer.setData("from-column-index", fromColumnIndex);
      e.dataTransfer.setData("type", "column");
    },
    createTask(e, tasks) {
      this.$store.commit("CREATE_TASK", {
        tasks,
        name: e.target.value
      });
      e.target.value = "";
    }
  }
};
</script>

<style lang="css" scoped>
.column {
  @apply bg-grey-light p-2 mr-4 text-left shadow rounded;
  min-width: 350px;
}
</style>