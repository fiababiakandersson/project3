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

    this.worker.bookings.forEach((booking) => {
      if (booking.status == 'Booked' && booking.percentage == 100) {
        this.url = baseUrl + booking.activity.toLowerCase() + '100.png'
      } else if (booking.status == 'Absent') {
        this.url = baseUrl + 'absent.png'
      } else if (booking.status == 'Preliminary') {
        if (booking.percentage == 100) {
          this.url = baseUrl + booking.activity.toLowerCase() + 'Prel100.png'
        } else if (booking.percentage == 50) {
          this.url = baseUrl + booking.activity.toLowerCase() + 'Prel50.png'
        }
      } else {
        this.url = baseUrl + 'tillgänglig.png'
      }
    })
  },
}
</script>

<template>
  <td :title="worker.activity" style="width: 50px !important; height: 40px !important">
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
