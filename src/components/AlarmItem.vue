<script setup>
    import RingtoneIcon from './icons/RingtoneIcon.vue';
</script>

<template>
    <div class="container">
        <div class="item">
            <div class="time">
                08 : 67
            </div>
            <div class="col">
                <div class="alarm-title">
                    <div class="name">
                        my alarm
                    </div>
                    <div class="toggle">

                    </div>
                </div>
                <div class="item-days">
                    <div v-for="(day, index) in itemDays">
                        <button class="item-day">
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

    display: flex;
    justify-content: center;
    align-items: center;

    /* warna kalo keselect
    border: 1px solid #907EEB;
    color: #907EEB; */
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