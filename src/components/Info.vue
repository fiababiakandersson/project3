<script>
import WeekNavigator from '../components/WeekNavigator.vue'

export default {
  data() {
    return {
      jobs: [
        { title: 'Snickare', imgUrl: '/icons/carpenter.png' },
        { title: 'Elektriker', imgUrl: '/icons/electrician.png' },
        { title: 'Rörmockare', imgUrl: '/icons/plumber.png' },
        { title: 'Målare', imgUrl: '/icons/painter.png' },
        { title: 'Murare', imgUrl: '/icons/mason.png' },
      ],
      availabilityInfo: [
        { title: 'Tillgänglig', imgUrl: '/icons/bookingsIcons/tillgänglig.png' },
        { title: 'Frånvaro', imgUrl: '/icons/bookingsIcons/absent.png' },
        { title: '100% bokad', imgUrl: '/icons/bookingsIcons/fullyBooked.png' },
        { title: '50% bokad', imgUrl: '/icons/bookingsIcons/halfBooked.png' },
        {
          title: '100% preliminärt bokad',
          imgUrl: '/icons/bookingsIcons/preliminaryBooked.png',
        },
        {
          title: '50% preliminärt bokad',
          imgUrl: '/icons/bookingsIcons/halfPreliminaryBooked.png',
        },
      ],
    }
  },
  components: {
    WeekNavigator,
  },
}
</script>

<template>
  <div
    style="
      display: flex;
      align-items: end;
      justify-content: space-between;
      margin: 2.5rem 2rem 0 2rem;
    "
  >
    <div style="display: flex; flex-direction: column; gap: 20px">
      <div style="display: flex; gap: 32px">
        <div
          v-for="job in jobs"
          :key="job.imgUrl"
          style="font-size: 14px; letter-spacing: 2%; display: flex; align-items: center; gap: 8px"
        >
          <img :src="job.imgUrl" :alt="job.title" style="width: 25px; height: 25px" />
          {{ job.title }}
        </div>
      </div>
      <div style="display: flex; gap: 32px">
        <div
          v-for="info in availabilityInfo"
          :key="info.imgUrl"
          style="font-size: 14px; letter-spacing: 2%; display: flex; align-items: center; gap: 8px"
        >
          <img :src="info.imgUrl" :alt="info.title" style="width: 20px; height: 20px" />
          {{ info.title }}
        </div>
      </div>
    </div>

    <WeekNavigator />
  </div>
</template>
