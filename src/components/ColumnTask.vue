<template>
  <AppDrop @drop="moveTaskOrColumn">
    <AppDrag
      class="task"
      :transferData="{
      type: 'task',
      fromColumnIndex: columnIndex,
      fromTaskIndex: taskIndex
  }"
    >
      <div @click="goToTask(task)">
        <span class="w-full flex-no-shrink font-bold">{{ task.name }}</span>
        <p class="w-full flex-no-shrink mt-1 text-sm" v-if="task.description">{{ task.description }}</p>
      </div>
    </AppDrag>
  </AppDrop>
</template>

<script>
import movingTasksAndColumnsMixin from "../mixins/movingTasksAndColumnsMixin.js";
import AppDrag from "../components/AppDrag.vue";
import AppDrop from "../components/AppDrop.vue";
export default {
  components: {
    AppDrop,
    AppDrag
  },
  mixins: [movingTasksAndColumnsMixin],
  props: {
    task: {
      type: Object,
      required: true
    },
    taskIndex: {
      type: Number,
      required: true
    }
  },
  methods: {
    goToTask(task) {
      this.$router.push({ name: "task", params: { id: task.id } });
    }
  }
};
</script>

<style scoped>
</style>