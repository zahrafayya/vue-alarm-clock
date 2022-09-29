<template>
    <div class="container">
        <div v-for="alarm in alarms">
            {{ alarm.hour }}, {{ alarm.minute }}
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            alarms: [
                { hour: 3, minute: 4 },
                { hour: 2, minute: 1 },
                { hour: 6, minute: 6 }
            ]
        }
    }
}
</script>

<style>

</style>