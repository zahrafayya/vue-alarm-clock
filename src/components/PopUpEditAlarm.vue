<script setup>
  import RingtoneIcon from './icons/RingtoneIcon.vue';
</script>

<template>
  <div class="popup">
    <div class="popup-inner">
      <div class="popup-text">
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
        <div class="flex margin-top">
          <div class="edit-section">
            <div class="add-label">Alarm Name: </div>
            <input class="alarm-name-input" v-model="alarmName"/>
          </div>
          <div class="edit-section">
            <div class="add-label">Ringtone: </div>
            <div class="select-ringtone">
              <div class="ringtone-label">Alarm ringtone</div>
            </div>
          </div>
        </div>

        <div class="flex">
          <div class="edit-section">
            <div class="add-label">Ring Duration: </div>
            <input class="alarm-name-input" v-model="ringDuration"/>
          </div>
          <div class="edit-section">
            <div class="add-label">Snooze Wait: </div>
            <input class="alarm-name-input" v-model="snoozeWait"/>
          </div>
        </div>

        <div class="days">
          <div v-for="(day, index) in dayTemp">
            <button class="item-day" @click="changeDay(index)"
                    :style="day ? { 'border': '1px solid #A663E5', 'color': '#A663E5' } : null">
              {{ this.days[index] }}
            </button>
          </div>
        </div>
      </div>
      <div>
        <button class="popup-close-1" @click="processEdit(); confirmEdit()">
          Apply
        </button>
        <button class="popup-close" @click="cancelEdit()">
          Cancel
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {confirmEdit: null, cancelEdit: null, alarm: null },
  data() {
    return {
      days: ["MO", "TU", "WE", "TH", "FR", "SA", "SU"],
      dayTemp: [],
      alarmName: this.alarm.name,
      ringDuration: this.alarm.ringingTime,
      snoozeWait: this.alarm.snoozeMinute,
      hour_1: Math.trunc(this.alarm.hour / 10),
      hour_2: this.alarm.hour % 10,
      minute_1: Math.trunc(this.alarm.minute / 10),
      minute_2: this.alarm.minute % 10
    };
  },
  methods : {
    changeDay(day) {
      this.dayTemp[day] = !this.dayTemp[day];
    },
    processEdit() {
      this.alarm.name = this.alarmName;
      this.alarm.hour = this.hour_1 * 10 + this.hour_2;
      this.alarm.minute = this.minute_1 * 10 + this.minute_2;
      for (let i = 0; i < this.alarm.days.length; i++) {
        this.alarm.days[i] = this.dayTemp[i];
      }
      this.alarm.ringingTime = this.ringDuration;
      this.alarm.snoozeMinute = this.snoozeWait;
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
  },
  mounted() {
    for (let i = 0; i < this.alarm.days.length; i++) {
      this.dayTemp.push(this.alarm.days[i]);
    }
  }
}

</script>

<style>
.flex {
  display: flex;
  margin-bottom: 24px;
  justify-content: space-around;
  align-items: center;
}

.margin-top {
  margin-top: 32px;
}

.popup {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 99;
  background-color: rgba(0, 0, 0, 0.2);

  display: flex;
  align-items: center;
  justify-content: center;
}

.popup-inner {
  display: flex;
  flex-direction: column;
  align-items: center;

  justify-content: center;
  background: #242424;
  color: #FFF;
  padding: 32px;

  border-radius: 12px;
}

.popup-close-1 {
  margin-right: 16px;
}

.edit-section {
  all: unset;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;

  margin-bottom: 24px;
}
</style>