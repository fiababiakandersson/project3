<script setup>
import { ref, onMounted } from 'vue';
import TableCell from './TableCell.vue';

const workers = ref([]);

// Fetch data from API
const fetchData = async () => {
    try {
        const response = await fetch('https://yrgo-web-services.netlify.app/bookings?start=2025-04-07&end=2025-05-02');
        const data = await response.json();

        // Process the API data into our table format
        workers.value = data.map(worker => ({
            id: worker.name,
            name: worker.name,
            professions: worker.professions,
            bookings: worker.bookings
        }));

    } catch (error) {
        console.error('Error fetching data:', error);
    }
};

onMounted(() => {
    fetchData();
});

// Helper functions to determine cell status
const getCellStatus = (worker, dayIndex) => {
    const targetDate = new Date(2025, 4, 1);
    targetDate.setDate(targetDate.getDate() + dayIndex);

    const booking = worker.bookings.find(b => {
        const fromDate = new Date(b.from);
        const toDate = new Date(b.to);
        return targetDate >= fromDate && targetDate <= toDate;
    });

    if (!booking) return { status: 'available' };

    return {
        status: booking.status.toLowerCase(),
        percentage: booking.percentage,
        activity: booking.activity
    };
};

// // Sample data: 28 workers
// const workers = ref(
//     Array.from({ length: 28 }, (_, i) => ({
//         id: i + 1,
//         name: `Worker ${i + 1}`,
//         schedule: Array(31).fill(false)
//     }))
// );


// Generate week headers (e.g., "v.1", "v.2", etc.)
const getWeekHeaders = () => {
    // Calculate how many week columns we need for 31 days
    // Need to handle 5 weeks (4 full weeks + partial 5th week)
    return ['v.1', 'v.2', 'v.3', 'v.4', 'v.5'];
};

// Generate day numbers (1-31)
const getDayNumbers = () => {
    return Array.from({ length: 31 }, (_, i) => i + 1);
};

// Swedish weekday abbreviations
const getWeekdayNames = () => {
    const days = [];
    const today = new Date(2025, 4, 1);
    today.setDate(today.getDate() - today.getDay() + 1);

    for (let i = -1; i < 30; i++) {
        const day = new Date(today);
        day.setDate(day.getDate() + i);
        const dayOfWeek = day.getDay();
        // Swedish weekday abbreviations
        const swedishDays = ['Mån', 'Tis', 'Ons', 'Tor', 'Fre', 'Lör', 'Sön'];
        days.push(swedishDays[dayOfWeek]);
    }


    return days;
};

// Add these methods inside your <script setup>
const isWeekend = (dayNumber) => {
    // Day 1 is Monday, so day 6 and 7 are weekend (Lör and Sön)
    return (dayNumber % 7 === 6 || dayNumber % 7 === 0);
};

const isWeekendDay = (dayName) => {
    return dayName === 'Lör' || dayName === 'Sön';
};
</script>

<template>
    <div class="booking-app">
        <table>
            <thead>
                <tr>
                    <th rowspan="3">Planering av: <br> <strong>Namn Namnsson</strong></th>
                    <th colspan="7">v.1</th>
                    <th colspan="7">v.2</th>
                    <th colspan="7">v.3</th>
                    <th colspan="7">v.4</th>
                    <th colspan="3">v.5</th>
                </tr>
                <tr>
                    <th v-for="day in getDayNumbers()" :key="day" :class="{ 'weekend': isWeekend(day) }">
                        {{ day }}
                    </th>
                </tr>
                <tr>
                    <th v-for="(day, index) in getWeekdayNames()" :key="index"
                        :class="{ 'weekend': isWeekendDay(day) }">
                        {{ day }}
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(worker, index) in workers" :key="worker.id" :class="{ 'even-row': index % 2 === 1 }">
                    <td>{{ worker.name }}</td>
                    <TableCell v-for="(day, dayIndex) in getDayNumbers()"
                        :activity="getCellStatus(worker, dayIndex).activity" :is-weekend="isWeekend(dayIndex + 1)" />
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
    font-size: small;
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
    background-color: #f0f0f0;
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

/* the weekend cells to stand out more in the worker rows */
/* :deep(.weekend) {
    background-color: #f0f0f0;
} */
</style>