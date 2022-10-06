<template>
  <div class="popup">
    <div class="popup-inner">
      <div class="popup-text">
        <slot />
      </div>
      <div>
        <button class="popup-close-1" @click="snoozeAlarm(); stopMusic()">
          Snooze Alarm
        </button>
        <button class="popup-close" @click="stopAlarm(); stopMusic()">
          Stop Alarm
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: { stopAlarm: null, snoozeAlarm: null, alarm: null},
  data () {
    return {
      ringtone: null,
      currMinutes: null,
      ringtoneList: ['../src/assets/audio/default_1.mp3', '../src/assets/audio/default_2.wav'],
    }
  },
  methods: {
    stopMusic() {
      this.ringtone.pause();
      this.currentTime = 0;
    },
    checkMinute() {
      const today = new Date();

      let _minutes = today.getMinutes();

      if((this.alarm.minute + this.alarm.ringingTime) % 60 === _minutes && !this.alarm.isSnoozed)
      {
        this.stopMusic();
        this.stopAlarm();
      }
    }
  },
  mounted() {
    const today = new Date();

    this.currMinutes = today.getMinutes();

    this.ringtone = new Audio(this.ringtoneList[this.alarm.ringtone]);
    if (typeof this.ringtone.loop == 'boolean')
    {
      this.ringtone.loop = true;
    }
    else
    {
      this.ringtone.addEventListener('ended', function() {
        this.currentTime = 0;
        this.play();
      }, false);
    }
    this.ringtone.play();

    setInterval(() => this.checkMinute(), 1000);
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
</style>