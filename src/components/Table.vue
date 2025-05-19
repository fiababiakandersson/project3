<script>
import TableCell from './TableCell.vue'

export default {
  components: { TableCell },

  data() {
    return {
      data: null, // holds fetched api data
      //   workers: Array.from({ length: 28 }, (_, i) => ({
      //     id: i + 1,
      //     name: `Worker ${i + 1}`,
      //     schedule: Array(31).fill(false),
      //   })),
    }
  },

  async created() {
    try {
      const res = await fetch('https://yrgo-web-services.netlify.app/bookings')
      const json = await res.json()
      this.data = json
    } catch (error) {
      console.error('Failed to fetch:', error)
    }
  },

  methods: {
    getDayNumbers() {
      return Array.from({ length: 31 }, (_, i) => i + 1)
    },
    getWeekHeaders() {
      return ['v.1', 'v.2', 'v.3', 'v.4', 'v.5']
    },
    getWeekdayNames() {
      const days = []
      const swedishDays = ['Mån', 'Tis', 'Ons', 'Tor', 'Fre', 'Lör', 'Sön']
      for (let i = 0; i < 31; i++) {
        days.push(swedishDays[i % 7])
      }
      return days
    },
    isWeekend(dayNumber) {
      return dayNumber % 7 === 6 || dayNumber % 7 === 0
    },
    isWeekendDay(dayName) {
      return dayName === 'Lör' || dayName === 'Sön'
    },
  },
}
</script>

<template>
  <div class="booking-app">
    <table>
      <thead>
        <tr>
          <th rowspan="3">
            Planering av: <br />
            <strong>Namn Namnsson</strong>
          </th>
          <th colspan="7">v.1</th>
          <th colspan="7">v.2</th>
          <th colspan="7">v.3</th>
          <th colspan="7">v.4</th>
          <th colspan="3">v.5</th>
        </tr>
        <tr>
          <th v-for="day in getDayNumbers()" :key="day" :class="{ weekend: isWeekend(day) }">
            {{ day }}
          </th>
        </tr>
        <tr>
          <th
            v-for="(day, index) in getWeekdayNames()"
            :key="index"
            :class="{ weekend: isWeekendDay(day) }"
          >
            {{ day }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(worker, i) in data" :key="i" :class="{ 'even-row': i % 2 === 1 }">
          <td
            style="
              display: flex;
              align-items: center;
              gap: 5px;
              justify-content: space-between;
              border-bottom: 0;
            "
          >
            {{ worker.name }}
            <div style="display: flex; gap: 9px">
              <img
                v-for="(job, idx) in worker.professions"
                :key="`${worker.id}-${job}-${idx}`"
                :src="`/icons/${job.toLowerCase()}.png`"
                @error="(e) => (e.target.src = '/icons/bookingsIcons/frånvaro.png')"
                :alt="job"
                width="20"
                height="20"
              />
            </div>
          </td>

          <TableCell
            v-for="day in getDayNumbers()"
            :worker="worker"
            :isWeekendDay="isWeekend(day)"
            :dayIndex="day - 1"
            :class="{ weekend: isWeekend(day) }"
          />
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.booking-app {
  overflow-x: auto;
  padding: 20px;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th {
  background-color: #ffffff;
  position: sticky;
  top: 0;
  text-align: left;
  padding-left: 15px;
}

/* Target just the first row of week headers specifically */
thead tr:first-child th {
  text-align: left;
  padding-left: 15px;
}

th,
td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: center;
  color: #949098;
}

.booked {
  background-color: #ffcccc;
  cursor: pointer;
}

.available {
  cursor: pointer;
}

th.weekend,
td.weekend {
  background-color: #e0e0e0;
  border-color: #ccd0d0;
  color: #949098;
}

/* Alternate row colors */
tbody tr.even-row {
  background-color: #f9f9f9;
}

/* Make sure TableCell content respects the weekend class */
:deep(.weekend .cell-content) {
  background-color: inherit;
  color: inherit;
}
</style>
