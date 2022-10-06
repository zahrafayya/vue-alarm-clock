<script setup>
    import AlarmItem from './AlarmItem.vue'
    import RingtoneIcon from './icons/RingtoneIcon.vue';
    import PopUpRinging from "./PopUpRinging.vue";
    import PopUpMaxSnooze from "./PopUpMaxSnooze.vue";
    import PopUpAdd from "./PopUpAdd.vue";
    import PopUpNoDay from "./PopUpNoDay.vue";
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
        <div class="alarm-item">
            <AlarmItem :alarms="alarms"/>
        </div>

        <PopUpRinging
            v-if="isRinging"
            :stopAlarm = "() => stopAlarm(currentRingingAlarm)"
            :snoozeAlarm = "() => snoozeAlarm(currentRingingAlarm)"
        >
            <p>{{ ringingAlarmName }} is ringing!</p>
        </PopUpRinging>

        <PopUpMaxSnooze v-if="hasReachedMaxSnooze"
        :maxSnoozeOk = "() => maxSnoozeOk()"
        />

        <PopUpAdd v-if="addAlarmSuccess"
        :addAlarmOk = "() => addAlarmOk()"
        >
          Alarm "{{ this.alarms[this.alarms.length - 1].name }}" has been successfully added.
        </PopUpAdd>

      <PopUpNoDay v-if="alarmNoDay"
                  :addAlarmNoDay = "() => addAlarmNoDay()"
      />
    </div>
</template>

<script>
export default {
    data() {
        return {
            snoozeMinute: 1,
            ringingTime: 2,
            hour_1: 0,
            hour_2: 0,
            minute_1: 0,
            minute_2: 0,
            isRinging: false,
            ringingAlarms: [], // id, endHour, endMinute
            snoozedAlarms: [], // id, pressSnoozeCount
            currentRingingAlarm: null,
            ringingAlarmName: null,

            hasReachedMaxSnooze: false,
            addAlarmSuccess: false,
            alarmNoDay: false,

            days: ["MO", "TU", "WE", "TH", "FR", "SA", "SU"],

            alarms: [{
                id: 0,
                name: null,
                hour: 0,
                minute: 0,
                days: [false, false, false, false, false, false, false],
                isSnoozed: false,
                isRinging: false,
                hasStopped: false,
                ringtone: null,
                daysLeft: 0,
                hourLeft: 0,
                minuteLeft: 0,
                isActive: true,
            }],

            alarmAtHour: {
              _0: [], _1: [], _2: [], _3: [], _4: [], _5: [], _6: [], _7: [], _8: [],
              _9: [], _10: [], _11: [], _12: [], _13: [], _14: [], _15: [], _16: [],
              _17: [], _18: [], _19: [], _20: [], _21: [], _22: [], _23: []

            },

            selectDay: [false, false, false, false, false, false, false],
            alarmName: null,


        };
    },
    methods: {
      changeDayToggle(day) {
        this.selectDay[day] = !this.selectDay[day];
      },
      addAlarmOk() {
        this.addAlarmSuccess = false;
      },
      maxSnoozeOk() {
        this.hasReachedMaxSnooze = false;
      },
      addAlarmNoDay() {
        this.alarmNoDay = false;
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
        } else {
          if (this.hour_2 == 0) {
            this.hour_2 = 9;
            this.hour_1--;
          } else
            this.hour_2--;
        }
      },
      upMinute() {
        if (this.minute_2 == 9) {
          this.minute_2 = 0;
          this.minute_1++;
        } else
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
        } else {
          if (this.minute_2 == 0) {
            this.minute_2 = 9;
            this.minute_1--;
          } else
            this.minute_2--;
        }
      },
      checkTime() {
        const today = new Date();
        let _hour = today.getHours();
        let _minute = today.getMinutes();
        let _day = today.getDay();

        for (let i = 0; i < this.alarmAtHour['_' + _hour].length; i++) {
          let _id = this.alarmAtHour['_' + _hour][i];
          let _alarm = this.alarms.find(a => a.id === _id);

          if (_alarm.days[_day - 1]) {
            if (_alarm.minute == _minute && !_alarm.hasStopped) {
              let _endMinute = ((_alarm.minute + this.ringingTime) % 60);
              let _endHour = ((_alarm.minute + this.ringingTime) >= 60) ?
                  ((_alarm.hour + 1) % 24) : _alarm.hour;

              _alarm.isRinging = true;

              this.addRingingAlarm(_alarm.id, _endHour, _endMinute);
            }
          }
        }
      },
      checkSnooze() {
        const today = new Date();
        let _hour = today.getHours();
        let _minute = today.getMinutes();

        for (let j = 0; j < this.snoozedAlarms.length; j++) {
          let _alarm = this.alarms.find(a => a.id === this.snoozedAlarms[j].id);

          let _minuteSnooze = ((_alarm.minute + (this.snoozeMinute * this.snoozedAlarms[j].pressSnoozeCount)) % 60);
          let _hourSnooze = ((_alarm.minute + (this.snoozeMinute * this.snoozedAlarms[j].pressSnoozeCount)) >= 60) ?
              ((_alarm.hour + 1) % 24) : _alarm.hour;

          console.log(_hour == _hourSnooze && _minute == _minuteSnooze);
          console.log(_hourSnooze);
          console.log(_minuteSnooze);
          console.log(this.snoozedAlarms[j].pressSnoozeCount);

          if (_hour == _hourSnooze && _minute == _minuteSnooze) {
            console.log("MASUK");
            let _endMinute = ((_minuteSnooze + this.ringingTime) % 60);
            let _endHour = ((_hourSnooze + this.ringingTime) >= 60) ?
                ((_alarm.hour + 1) % 24) : _alarm.hour;

            _alarm.isRinging = true;

            this.addRingingAlarm(_alarm.id, _endHour, _endMinute);

            console.log("TESTING");
          }
        }
      },
      addRingingAlarm(id, stop_hour, stop_minute) {
        this.ringingAlarms.push({
          id: id,
          endHour: stop_hour,
          endMinute: stop_minute
        });
      },
      playAlarm() {
        for (let j = 0; j < this.ringingAlarms.length; j++) {
          const today = new Date();
          let _hour = today.getHours();
          let _minute = today.getMinutes();

          let _alarm = this.alarms.find(a => a.id === this.ringingAlarms[j].id);

          if ((_hour > this.ringingAlarms.endHour) ||
              (_hour === this.ringingAlarms.endHour && _minute >= this.ringingAlarms.endMinute)) {
            this.stopAlarm(_alarm.id);

            // rollback
            _alarm.hasStopped = false;
            this.ringingAlarms.splice(j, 1);
          }

          if (_alarm.isRinging) this.startAlarm(_alarm.id);
        }

      },
      startAlarm(id) {
        let _alarm = this.alarms.find(a => a.id === id);

        this.isRinging = true;
        this.currentRingingAlarm = id;
        this.ringingAlarmName = _alarm.name;
      },
      stopAlarm(id) {
        let _alarm = this.alarms.find(a => a.id === id);

        this.isRinging = false;
        this.currentRingingAlarm = null;
        this.ringingAlarmName = null;

        _alarm.isRinging = false;
        _alarm.hasStopped = true;
      },
      snoozeAlarm(id) {
        let _alarm = this.alarms.find(a => a.id === id);
        let _snoozedAlarm = this.snoozedAlarms.find(a => a.id === id);

        if (_snoozedAlarm) {
          if (_snoozedAlarm.pressSnoozeCount == 3) {
            _alarm.isSnoozed = false;
            let _index = this.snoozedAlarms.findIndex(obj => obj.id === id);
            this.snoozedAlarms.splice(_index, 1);

            // kasih popup tulisan kalau alarm udah ga bisa disnooze lagi
            this.hasReachedMaxSnooze = true;

            this.stopAlarm(id);
          } else _snoozedAlarm.pressSnoozeCount++;
        } else {
          _alarm.isSnoozed = true;
          this.snoozedAlarms.push({
            id: id,
            pressSnoozeCount: 1
          });
        }

        this.stopAlarm(id);

      },
      addAlarm() {
        let _hour = this.hour_1 * 10 + this.hour_2;
        let _minute = this.minute_1 * 10 + this.minute_2;

        console.log(this.selectDay.includes(true));

        if (this.selectDay.includes(true))
        {
          this.alarms.push({
            id: this.alarms[this.alarms.length - 1].id + 1,
            name: (this.alarmName) ? this.alarmName : "My Alarm " + (this.alarms[this.alarms.length - 1].id + 1),
            hour: _hour,
            minute: _minute,
            days: this.selectDay,
            isSnoozed: false,
            isRinging: false,
            hasStopped: false,
            ringtone: null,
            daysLeft: 0,
            hourLeft: 0,
            minuteLeft: 0,
            isActive: true
          });
          this.alarmAtHour['_' + _hour].push(this.alarms[this.alarms.length - 1].id);

          this.addAlarmSuccess = true;
        }
        else
        {
          this.alarmNoDay = true;
        }


      },
    },
    mounted() {
        this.alarms = JSON.parse(localStorage.getItem('alarm')) || this.alarms;
        setInterval(() => this.checkTime(), 1000);
        setInterval(() => this.playAlarm(), 1000);
        setInterval(() => this.checkSnooze(), 3000);
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
.alarm-item {
    margin-bottom: 72px;
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
    font-size: 24px;
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
    font-size: 21px;
    border-radius: 100%;
    border: 2px solid #FFF;
    width: 45px;
    height: 45px;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-left: 6px;
    margin-right: 6px;
}

.day:hover {
    border: 2px solid rgb(65, 65, 65);
    color: rgb(65, 65, 65);
    cursor: context-menu;
}

</style>