<script>
export default {
  props: {
    worker: {
      type: Object,
      required: true,
    },
    isWeekendDay: {
      type: Boolean,
      default: false,
    },
    dayIndex: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      url: '',
    }
  },
  created() {
    const baseUrl = '/icons/bookingsIcons/'

    this.url = baseUrl + 'tillgänglig.png'
  },
}
</script>

<template>
  <td :title="worker.activity">
    <img v-if="!this.isWeekendDay" :src="url" :alt="worker.activity" width="20" height="20" />
  </td>
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
