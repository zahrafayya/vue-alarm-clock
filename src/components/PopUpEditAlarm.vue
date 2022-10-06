<script setup>
  import RingtoneIcon from './icons/RingtoneIcon.vue';
</script>

<template>
  <div class="popup">
    <div class="popup-inner">
      <div class="popup-text">
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
        <div class="edit-section">
          <div class="add-label">Ring Duration: </div>
          <input class="alarm-name-input" v-model="ringDuration"/>
        </div>
        <div class="edit-section">
          <div class="add-label">Snooze Wait: </div>
          <input class="alarm-name-input" v-model="snoozeWait"/>
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
      snoozeWait: this.alarm.snoozeMinute
    };
  },
  methods : {
    changeDay(day) {
      this.dayTemp[day] = !this.dayTemp[day];
    },
    processEdit() {
      this.alarm.name = this.alarmName;
      for (let i = 0; i < this.alarm.days.length; i++) {
        this.alarm.days[i] = this.dayTemp[i];
      }
      this.alarm.ringingTime = this.ringDuration;
      this.alarm.snoozeMinute = this.snoozeWait;
    }
  },
  mounted() {
    for (let i = 0; i < this.alarm.days.length; i++) {
      this.dayTemp.push(this.alarm.days[i]);
    }
  }
}

</script>

<style>
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

.popup-text {
  margin-bottom: 24px;
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

  margin-bottom: 32px;
}
</style>