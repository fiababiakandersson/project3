<script>
export default {
  props: {
    worker: {
      activity: String,
      percentage: Number,
      status: String,
      from: String,
      to: String,
    },
    isWeekend: Boolean,
    isEvenRow: Boolean,
  },
}
</script>

<template>
  <td
    :class="['status-indicator', status, jobType, { weekend: isWeekend, 'even-row': isEvenRow }]"
    :title="worker.activity"
  ></td>
</template>

<style>
/* Table styling */
td {
  padding: 4px;
  border: 1px solid #ddd;
  text-align: center;
}

.weekend {
  background-color: #f5f5f5;
}

.even-row:not(.weekend) {
  background-color: #f9f9f9;
}
</style>
