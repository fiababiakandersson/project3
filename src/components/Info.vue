<script>
export default {
  data() {
    return {
      jobs: [
        { title: 'Snickare', imgUrl: '../../public/icons/snickare.png' },
        { title: 'Elektriker', imgUrl: '../../public/icons/elektriker.png' },
        { title: 'Rörmockare', imgUrl: '../../public/icons/rörmockare.png' },
        { title: 'Målare', imgUrl: '../../public/icons/målare.png' },
        { title: 'Murare', imgUrl: '../../public/icons/murare.png' },
      ],
      availabilityInfo: [
        'Tillgänglig',
        'Frånvaro',
        '100% bokad',
        '50% bokad',
        '100% preliminärt bokad',
        '50% preliminärt bokad',
      ],
    }
  },
}
</script>

<template>
  <div style="width: 60%; display: flex; flex-direction: column; gap: 10px">
    <div style="display: flex; gap: 25px">
      <div
        v-for="job in jobs"
        :key="job.imgUrl"
        style="font-size: 14px; letter-spacing: 2%; display: flex; align-items: center; gap: 8px"
      >
        <img :src="job.imgUrl" :alt="job.title" style="width: 25px; height: 25px" />
        {{ job.title }}
      </div>
    </div>
    <div style="display: flex; gap: 15px">
      <div v-for="info in availabilityInfo" style="font-size: 14px; letter-spacing: 2%">
        {{ info }}
      </div>
    </div>
  </div>
</template>
