<!--
/**
 * @fileoverview TaskList component
 * @license MIT
 * @author Rafal Pospiech <neuronet.io@gmail.com>
 * @package GanttElastic
 */
-->
<template>
  <div
    class="gantt-elastic__task-list-wrapper"
    ref="taskListWrapper"
    :style="{ ...root.style['task-list-wrapper'], width: '100%', height: '100%' }"
    v-show="root.state.options.taskList.display"
  >
    <div class="gantt-elastic__task-list" :style="{ ...root.style['task-list'] }" ref="taskList">
      <task-list-header></task-list-header>
      <div
        class="gantt-elastic__task-list-items"
        ref="taskListItems"
        :style="{ ...root.style['task-list-items'], height: root.state.options.rowsHeight + 'px' }"
      >
        <task-list-item v-for="task in visibleTaskList" :key="task.task.id" :task="task.task" :style="task.style"></task-list-item>
      </div>
    </div>
  </div>
</template>

<script>
import TaskListHeader from './TaskListHeader.vue';
import TaskListItem from './TaskListItem.vue';
export default {
  name: 'TaskList',
  components: {
    TaskListHeader,
    TaskListItem
  },
  inject: ['root'],
  data() {
    return {};
  },

  /**
   * Mounted
   */
  mounted() {
    this.root.state.refs.taskListWrapper = this.$refs.taskListWrapper;
    this.root.state.refs.taskList = this.$refs.taskList;
    this.root.state.refs.taskListItems = this.$refs.taskListItems;
  },

  computed: {
    visibleTaskList() {
      let ret = [];
      var lastUser = this.root.visibleTasks[0].user;
      for (let index = 0; index < this.root.visibleTasks.length; ++index) {
        let task = this.root.visibleTasks[index];
        let style = task.user == lastUser ? {} : {'border-top': '1px solid #F0F'};
        lastUser = task.user;
        ret.push({
          task: task,
          style: style,
        });
      }
      return ret;
    },
  }
};
</script>
