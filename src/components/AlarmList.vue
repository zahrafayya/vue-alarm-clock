<script setup>
    import AlarmItem from './AlarmItem.vue'
    import RingtoneIcon from './icons/RingtoneIcon.vue';
</script>

<template>
    <div class="container">
        <div class="set">
            <div class="set-clock">
                <div class="hour">
                    <button class="button up" @click="upHour">
                        ^
                    </button>
                    <div class="time-display">
                        <div class="time-digit">
                        {{ hour_1 }}
                    </div>
                        <div class="time-digit">
                            {{ hour_2 }}
                        </div>
                    </div>
                    <button class="button down" @click="downHour">
                        v
                    </button>
                </div>
                <div class="titikdua">
                    :
                </div>
                <div class="minute">
                    <button class="button up" @click="upMinute">
                        ^
                    </button>
                    <div class="time-display">
                        <div class="time-digit">
                        {{ minute_1 }}
                    </div>
                        <div class="time-digit">
                            {{ minute_2 }}
                        </div>
                    </div>
                    <button class="button down" @click="downMinute">
                        v
                    </button>
                </div>
            </div>
            <div class="alarm-details">
                <div class="alarm-name">
                    <div class="add-label">Alarm Name: </div>
                    <input class="alarm-name-input" v-model="alarmName" placeholder="Alarm Name" />
                </div>
                <div class="alarm-ring">
                    <div class="add-label">Ringtone: </div>
                    <div class="select-ringtone">
                        <RingtoneIcon/>
                        <div class="ringtone-label">Alarm ringtone</div>
                    </div>
                </div>
                <div class="days">
                    <div v-for="(day, index) in days">
                        <button class="day" @click="changeDayToggle(index)" 
                        :style=" selectDay[index] ? { 'color': '#000', 'border': '1px solid #000' } : null">
                        {{ day }}
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <button class="add" @click="addAlarm">
            Add
        </button>
        <AlarmItem :alarms="alarms"/>
        <AlarmItem/>
        <div v-for="alarm in alarms">
            <p>aaaaaaaa</p>
            <p>aaaaaaaa</p>
            <p>aaaaaaaa</p>
            <div v-if="alarm.id">
                {{ alarm.hour }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            hour_1: 0,
            hour_2: 0,
            minute_1: 0,
            minute_2: 0,
            isRinging: false,

            days: ["MO", "TU", "WE", "TH", "FR", "SA", "SU"],

            alarms: [{ 
                id: 0, 
                name: null,
                hour: 0, 
                minute: 0, 
                days: [false, false, false, false, false, false, false], 
                ringtone: null, 
                isSnooze: false, 
                isStopped: false,
                daysLeft: 0, 
                hourLeft: 0, 
                minuteLeft: 0,
            }],

            selectDay: [false, false, false, false, false, false, false],
            alarmName: null
        };
    },
    methods: {
        changeDayToggle(day) {
            this.selectDay[day] = !this.selectDay[day];
        },
        upHour() {
            if (this.hour_2 == 9)
                this.hour_1++;
            this.hour_2 = (this.hour_2 == 9) ? 0 : this.hour_2 + 1;
            if (this.hour_1 == 3)
                this.hour_1 = 0;
            if (this.hour_1 == 2 && this.hour_2 == 4) {
                this.hour_1 = 0;
                this.hour_2 = 0;
            }
        },
        downHour() {
            if (this.hour_1 == 0 && this.hour_2 == 0) {
                this.hour_1 = 2;
                this.hour_2 = 3;
            }
            else {
                if (this.hour_2 == 0) {
                    this.hour_2 = 9;
                    this.hour_1--;
                }
                else
                    this.hour_2--;
            }
        },
        upMinute() {
            if (this.minute_2 == 9) {
                this.minute_2 = 0;
                this.minute_1++;
            }
            else
                this.minute_2++;
            if (this.minute_1 == 6) {
                this.minute_1 = 0;
                this.upHour();
            }
        },
        downMinute() {
            if (this.minute_1 == 0 && this.minute_2 == 0) {
                this.downHour();
                this.minute_1 = 5;
                this.minute_2 = 9;
            }
            else {
                if (this.minute_2 == 0) {
                    this.minute_2 = 9;
                    this.minute_1--;
                }
                else
                    this.minute_2--;
            }
        },
        checkTime() {
            const today = new Date();
            let _hour = today.getHours();
            let _minute = today.getMinutes();
            if (_hour == (this.hour_1 * 10) + this.hour_2 &&
                _minute == (this.minute_1 * 10) + this.minute_2) {
                if (!this.ring) {
                    alert("Alarm is ringing!");
                    this.ring = true;
                }
            }
            else
                this.ring = false;
        },
        addAlarm() {
            let _hour = this.hour_1 * 10 + this.hour_2;
            let _minute = this.minute_1 * 10 + this.minute_2;
            this.alarms.push({ 
                id: this.alarms[this.alarms.length - 1].id + 1, 
                name: this.alarmName,
                hour: _hour, 
                minute: _minute,
                days: this.selectDay,

                ringtone: null, 
                isSnooze: false, 
                isStopped: false,
                daysLeft: 0, 
                hourLeft: 0, 
                minuteLeft: 0,
            });
            console.log(this.alarms);
        }
    },
    mounted() {
        this.alarms = JSON.parse(localStorage.getItem('alarm')) || this.alarms;
        setInterval(() => this.checkTime(), 1000);
    },
    watch: {
        alarms: {
            handler() {
                localStorage.setItem('alarm', JSON.stringify(this.alarms));
            },
            deep: true
        }
    }
}
</script>
    
<style>
.container
{
    display: flex;
    flex-direction: column;
    align-items: center;
}
.set
{
    background: linear-gradient(135deg, #f403d1, #64b5f6);
    border-radius: 12px;
    padding: 36px 48px;
    display: flex;
}
.set-clock {
    display: flex;
    align-items: center;
    justify-content: center;
}
.alarm-details {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    border-left: 1px solid #FFF;
    padding-left: 42px;
    margin-left: 42px;
}
.titikdua
{
    font-size: 64px;
    font-weight: 800;
    font-family: 'Reem Kufi Fun', sans-serif;
    padding-bottom: 24px;
    margin: 0 8px;
}
.days {
    display: flex;
    justify-content: center;
    margin-top: 20px;
}
.time-display
{
    display: flex;
}
.time-digit
{
    font-size: 64px;
    background: #1A1A1A;
    color: #FFF;
    display: flex;
    padding: 0 16px;
    height: 100px;
    justify-content: center;
    align-items: center;
    margin: 12px 4px;
    border-radius: 4px;

}
.add
{
    margin-top: 28px;
    display: flex;
    font-weight: 600;
    font-size: 24px;
    justify-content: center;
    align-items: center;
    width: 132px;
    height: 42px;
    border-radius: 18px;
    border: #A663E5 1px solid;
}
.alarm-name {
    margin-bottom: 20px;
}
.alarm-ring {
    margin-bottom: 8px;
}
.add-label {
    font-size: 21px;
    font-weight: 600;
    margin-bottom: 16px;
}
.select-ringtone {
    display: flex;
}
.ringtone-label {
    margin-left: 18px;
}
.day {
    all: unset;
    font-weight: 600;
    font-size: larger;
    border-radius: 100%;
    border: 1px solid #FFF;
    width: 42px;
    height: 42px;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-left: 6px;
    margin-right: 6px;
}

.day:hover {
    border: 1px solid rgb(65, 65, 65);
    color: rgb(65, 65, 65);
    cursor: context-menu;
}

</style>