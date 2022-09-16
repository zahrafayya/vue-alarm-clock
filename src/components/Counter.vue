<template>
    <div>
        {{displayHours}}
        {{displayMinutes}}
        {{displaySeconds}}
    </div>
</template>

<script>
export default {
    data: ()=>({
        displayHours: 0,
        displayMinutes: 0,
        displaySeconds: 0
    }),
    methods: {
        showTime() {
            
        }
    },
}
</script>

<style lang="scss" scoped>
</style>