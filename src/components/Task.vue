<template>
  <div :class="classes">
    <label :for="'checked' + task.id" :aria-label="'archiveTask-' + task.id" class="checkbox">
      <input type="checkbox" :checked="isChecked" disabled :name="'checked' + task.id" :id="'archiveTask-' + task.id" />
      <span class="checkbox-custom" @click="archiveTask" />
    </label>
    <label :for="'title-' + task.id" :aria-label="task.title" class="title">
      <input type="text" readonly :value="task.title" :id="'title-' + task.id" name="title" placeholder="Input Title" style="text-overflow: ellipsis;" />
    </label>
    <button v-if="!isChecked" class="pin-button" @click="pinTask" :id="'pinTask-' + task.id" :aria-label="'pinTask-' + task.id">
      <span class="icon-star" />
    </button>
  </div>
</template>

<script>
import {computed, reactive} from "vue";

export default {
  name: 'Task',
  props: {
    task: {
      type: Object,
      required: true,
      default: () => ({ id: '', state: '', title: '' }),
      validator: (task) => ['id', 'state', 'title'].every((key) => key in task)
    }
  },
  emits: ['archive-task', 'pin-task'],

  setup(props, { emit}) {
    props = reactive(props)
    return {
      classes: computed(() => ({
        'list-item TASK_INBOX': props.task.state === 'TASK_INBOX',
        'list-item TASK_PINNED': props.task.state === 'TASK_PINNED',
        'list-item TASK_ARCHIVED': props.task.state === 'TASK_ARCHIVED',
      })),
      isChecked: computed(() => props.task.state === 'TASK_ARCHIVED'),
      archiveTask() { emit('archive-task', props.task.id); },
      pinTask() { emit('pin-task', props.task.id); },
    }
  }
}
</script>
