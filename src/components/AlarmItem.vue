<script setup>
    import RingtoneIcon from './icons/RingtoneIcon.vue';
</script>

<template>
    <div class="container" v-for="alarm in alarms">
        <div class="item" v-if="alarm.id">
            <div class="time">
                {{ (alarm.hour < 10) ? '0' + alarm.hour : alarm.hour }} : {{ (alarm.minute < 10) ? '0' + alarm.minute : alarm.minute }}
            </div>
            <div class="col">
                <div class="alarm-title">
                    <div class="name">
                        {{ alarm.name }}
                    </div>
                    <div class="toggle">

                    </div>
                </div>
                <div class="item-days">
                    <div v-for="(day, index) in itemDays">
                        <button class="item-day" @click="changeDayItem(index, alarm.id)" 
                        :style=" alarm.days[index] ? { 'border': '1px solid #A663E5', 'color': '#A663E5' } : null">
                            {{ day }}
                        </button>
                    </div>
                </div>
                <div class="ringtone">
                    <RingtoneIcon/>
                    <div class="item-label">
                        ringtone
                    </div>
                </div>
                <div class="counter">
                    2 hours and 3 minutes left
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: { alarms: [] },
    data() {
        return {
            itemDays: ["MO", "TU", "WE", "TH", "FR", "SA", "SU"]
        }
    },
    methods: {
        changeDayItem(day, id) {
          let _alarm = this.alarms.find(a => a.id === id);

          _alarm.days[day] = !_alarm.days[day];
        },
        updateTimeLeft() {
          const today = new Date();

          const endDate = new Date(today.getDay());
          const days = parseInt((endDate - today) / (1000 * 60 * 60 * 24));
          const hours = parseInt(Math.abs(endDate - today) / (1000 * 60 * 60) % 24);
          const minutes = parseInt(Math.abs(endDate.getTime() - today.getTime()) / (1000 * 60) % 60);
          const seconds = parseInt(Math.abs(endDate.getTime() - today.getTime()) / (1000) % 60);
        }
    }
}
</script>

<style>
.item {
    border-radius: 12px;
    border: #BE46DF 2px solid;
    width: 40vw;
    height: fit-content;
    display: flex;
    flex-direction: row;
    align-items: center;

    margin-top: 36px;
    padding: 24px 30px;
}

.time {
    font-size: 64px;
    font-weight: 300;
    font-family: 'Reem Kufi Fun', sans-serif;
}

.col {
    margin-left: 32px;
    flex-grow: 1;

}

.alarm-title {
    display: flex;
}

.name {
    font-size: 28px;
    font-family: 'Poppins';
}

.item-days {
    margin-top: 24px;
    margin-bottom: 12px;

    display: flex;
}

.item-day {
    all: unset;
    font-weight: 600;
    border: 1px solid #FFF;
    border-radius: 100%;
    width: 36px;
    height: 36px;
    margin-right: 10px;
    cursor: context-menu;

    display: flex;
    justify-content: center;
    align-items: center;
}

.item-day:hover {
    color: #67B2F5;
    border: 1px solid #67B2F5;

}

.ringtone {
    display: flex;
    margin-top: 18px;
}

.item-label {
    margin-left: 12px;
}

.counter {
    /* display: flex; */
    margin-top: 18px;
    padding-top: 12px;
    font-weight: bolder;
    width: 100%;

    border-top: 1px solid #FFF;
} 
    
</style>