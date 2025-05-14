<script setup>
const props = defineProps({
    isWeekend: Boolean
});

const getCellClass = () => {
    if (props.isWeekend) return 'weekend';
};


</script>

<template>
    <td :class="['status-indicator', getCellClass()]"></td>
</template>

<style scoped>

.weekend {
    background-color: #f5f5f5;
}

</style>