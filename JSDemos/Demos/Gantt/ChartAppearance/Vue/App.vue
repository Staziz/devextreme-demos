<template>
  <div id="form-demo">
    <div class="options">
      <div class="caption">Options</div>
      <div class="option">
        <span>Scale Type</span>
        <DxSelectBox
          :items="['auto', 'minutes', 'hours', 'days', 'weeks', 'months', 'quarters', 'years']"
          v-model:value="scaleType"
        />
      </div>
      <div class="option">
        <span>Title Position</span>
        <DxSelectBox
          :items="['inside', 'outside', 'none']"
          v-model:value="taskTitlePosition"
        />
      </div>
      <div class="option">
        <DxCheckBox
          v-model:value="showResources"
          text="Show Resources"
        />
      </div>
      <div class="option">
        <DxCheckBox
          text="Customize Task Tooltip"
          v-model:value="showCustomTaskTooltip"
        />
      </div>
    </div>
    <div class="widget-container">
      <DxGantt
        :task-list-width="500"
        :height="700"
        :task-title-position="taskTitlePosition"
        :scale-type="scaleType"
        :show-resources="showResources"
        :task-tooltip-content-template="showCustomTaskTooltip ? 'taskTooltipContentTemplate' : ''"
      >

        <DxTasks :data-source="tasks"/>
        <DxDependencies :data-source="dependencies"/>
        <DxResources :data-source="resources"/>
        <DxResourceAssignments :data-source="resourceAssignments"/>

        <DxEditing :enabled="true"/>

        <DxColumn
          :width="300"
          data-field="title"
          caption="Subject"
        />
        <DxColumn
          data-field="start"
          caption="Start Date"
        />
        <DxColumn
          data-field="end"
          caption="End Date"
        />
        <template #taskTooltipContentTemplate="{ data: task }">
          <div class="custom-task-edit-tooltip">
            <div class="custom-tooltip-title">{{ task.title }}</div>
            <div class="custom-tooltip-row"><span> Estimate: </span>{{ getTimeEstimate(task) }}<span> hours </span></div>
            <div class="custom-tooltip-row"><span> Left: </span>{{ getTimeLeft(task) }}<span> hours </span></div>
          </div>
        </template>
      </DxGantt>
    </div>
  </div>
</template>
<script>
import {
  DxGantt,
  DxTasks,
  DxDependencies,
  DxResources,
  DxResourceAssignments,
  DxColumn,
  DxEditing
} from 'devextreme-vue/gantt';
import DxCheckBox from 'devextreme-vue/check-box';
import DxSelectBox from 'devextreme-vue/select-box';

import {
  tasks,
  dependencies,
  resources,
  resourceAssignments
} from './data.js';

export default {
  components: {
    DxGantt,
    DxTasks,
    DxDependencies,
    DxResources,
    DxResourceAssignments,
    DxColumn,
    DxEditing,
    DxCheckBox,
    DxSelectBox
  },
  data() {
    return {
      tasks: tasks,
      dependencies: dependencies,
      resources: resources,
      resourceAssignments: resourceAssignments,
      scaleType: 'months',
      taskTitlePosition: 'outside',
      showResources: true,
      showCustomTaskTooltip: true
    };
  },
  methods: {
    getTimeEstimate(task) {
      return Math.abs(task.start - task.end) / 36e5;
    },
    getTimeLeft(task) {
      const timeEstimate = Math.abs(task.start - task.end) / 36e5;
      return Math.floor((100 - task.progress) / 100 * timeEstimate);
    }
  }
};
</script>
<style>
  #gantt {
    height: 700px;
  }

  .options {
    margin-bottom: 20px;
    padding: 20px;
    background-color: rgba(191, 191, 191, 0.15);
    position: relative;
  }

  .caption {
    font-size: 18px;
    font-weight: 500;
  }

  .option {
    margin-top: 10px;
    margin-right: 44px;
    display: inline-block;
  }

  .option:last-child {
    margin-right: 0;
  }

  .custom-task-edit-tooltip {
      background-color: white;
      color: black;
      box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
      padding: 10px 12px 12px 12px;
      border-radius: 3px;
  }

  .custom-task-edit-tooltip::before {
      border-top-color: white;
      box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  }

  .custom-task-edit-tooltip::after {
      border-bottom-color: white;
  }

  .custom-tooltip-title {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      font-size: 13px;
      font-weight: 600;
      padding-bottom: 6px;
  }

  .custom-tooltip-row {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      font-size: 12px;
      font-weight: 600;
  }
</style>
