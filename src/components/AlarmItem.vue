<script setup>
    import RingtoneIcon from './icons/RingtoneIcon.vue';
    import ToggleButton from "./ToggleComponent.vue";
    import PencilIcon from "./icons/PencilIcon.vue";
    import TrashCanIcon from "./icons/TrashCanIcon.vue";
    import PopUpDeleteAlarm from "./PopUpDeleteAlarm.vue";
    import PopUpEditAlarm from "./PopUpEditAlarm.vue";
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
                      <div>
                        {{ alarm.name }}
                      </div>
                      <PencilIcon class="click-icon" @click="editAlarm(alarm.id)"/>
                    </div>
                    <div class="toggle">
                      <TrashCanIcon class="click-icon-2" @click="deleteAlarm(alarm.id)"/>
                      <ToggleButton
                          :activeAlarm = "() => activeAlarm(alarm.id)"
                          @click="checkActive(alarm.id)"
                      />
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

                </div>
            </div>

          <PopUpDeleteAlarm v-if="deleteAsk"
                            :confirmDelete="() => confirmDelete(idDelete)"
                            :cancelDelete="() => cancelDelete()">
            Are you sure you want to delete {{ alarms.find(a => a.id === idDelete).name }}?
          </PopUpDeleteAlarm>

          <PopUpEditAlarm v-if="clickEdit"
                          :confirmEdit="() => confirmEdit(idEdit)"
                          :cancelEdit="() => cancelEdit()"
                          :alarm="alarms.find(a => a.id === idEdit)"/>

        </div>
    </div>
</template>

<script>
export default {
    props: { alarms: [], snoozedAlarms: null, ringingAlarms: null },
    data() {
        return {
            itemDays: ["MO", "TU", "WE", "TH", "FR", "SA", "SU"],
            deleteAsk: false,
            clickEdit: false,
            idDelete: null,
            idEdit: null,
            daysLeft: null,
            hoursLeft: null,
            minutesLeft: null,
            secondsLeft: null
        }
    },
    methods: {
        checkActive(id) {
          let _indexRings = this.ringingAlarms.findIndex(obj => obj.id === id);
          let _indexSnooze = this.snoozedAlarms.findIndex(obj => obj.id === id);
          let _alarm = this.alarms.find(a => a.id === id);

          if(_alarm.isActive == false)
          {
            if(_indexRings)
            {
              this.ringingAlarms.splice(_indexRings, 1);
            }
            if(_indexSnooze)
            {
              this.snoozedAlarms.splice(_indexSnooze, 1);
            }

            _alarm.isSnoozed = false;
            _alarm.isRinging = false;
            _alarm.hasStopped = false;
          }
        },
        changeDayItem(day, id) {
          let _alarm = this.alarms.find(a => a.id === id);

          _alarm.days[day] = !_alarm.days[day];
        },
        activeAlarm(id) {
          let _alarm = this.alarms.find(a => a.id === id);

          _alarm.isActive = !_alarm.isActive;

          console.log(_alarm.isActive);
          console.log(_alarm);
          console.log(_alarm.id);
        },
        deleteAlarm(id) {
          this.deleteAsk = true;
          this.idDelete = id;
        },
        confirmDelete(id) {
          let _index = this.alarms.findIndex(obj => obj.id === id);
          this.alarms.splice(_index, 1);

          this.deleteAsk = false;
        },
        cancelDelete() {
          this.deleteAsk = false;
        },
        editAlarm(id) {
          this.clickEdit = true;
          this.idEdit = id;
        },
        confirmEdit(id) {
          this.clickEdit = false;
        },
        cancelEdit() {
          this.clickEdit = false;
        },
        updateTimeLeft(id) {
          const today = new Date();
          let _alarm = this.alarms.find(a => a.id === id);
          let _endDay = 100;

          for(let i = 0; i < 7; i ++){
            if(_alarm.days[i]) {
              let temp = (i+1) - today.getDay();
              if(temp < 0) temp = 7 + temp;
              if (_endDay > temp) _endDay = temp;
            }
          }

          const _date = new Date();
          _date.setDate(_date.getDate() + _endDay);

          let delta = 0;

          console.log(Date.parse(Math.abs(new Date(_date) - new Date())));


          this.daysLeft = Math.trunc(delta / 86400);
          delta -= this.daysLeft * 86400;

          console.log(this.daysLeft * 86400);


          this.hoursLeft = Math.floor(delta / 3600) % 24;
          delta -= this.hoursLeft * 3600;

          this.minutesLeft = Math.floor(delta / 60) % 60;
          delta -= this.minutesLeft * 60;

          this.secondsLeft = delta % 60;


          this.daysLeft = (this.daysLeft == 0) ? null : (this.daysLeft == 1) ? this.daysLeft + ' day' : this.daysLeft + ' days';
          this.hoursLeft = (this.hoursLeft == 0) ? null : (this.hoursLeft == 1) ? this.hoursLeft + ' hour' : this.hoursLeft + ' hours';
          this.minutesLeft = (this.minutesLeft == 0) ? null : (this.minutesLeft == 1) ? this.minutesLeft + ' minute' : this.minutesLeft + ' minutes';
          this.secondsLeft = (this.secondsLeft == 0) ? null : (this.secondsLeft == 1) ? this.secondsLeft + ' second' : this.secondsLeft + ' seconds';

         }
    },
    mounted() {
      // setInterval(() => this.updateTimeLeft(1), 1000);
    }
}
</script>

<style>
.toggle {
  display: flex;
}

.click-icon {
  margin-left: 21px;
  cursor: pointer;
}

.click-icon-2 {
  margin-right: 21px;
  cursor: pointer;
}

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
    justify-content: space-between;
}

.name {
    font-size: 28px;
    font-family: 'Poppins';
    display: flex;
}

.item-days {
    margin-top: 16px;
    margin-bottom: 12px;

    display: flex;
}

.item-day {
    all: unset;
    font-weight: 600;
    font-size: 18px;
    border: 1px solid #FFF;
    border-radius: 100%;
    width: 42px;
    height: 42px;
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
    margin-top: 20px;
}

.item-label {
    margin-left: 12px;
}

.counter {
    /* display: flex; */
    margin-top: 20px;
    padding-top: 12px;
    font-weight: bolder;
    width: 100%;

    border-top: 1px solid #FFF;
} 
    
</style>