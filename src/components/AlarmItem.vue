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
                      <PencilIcon class="click-icon" @click="editAlarm()"/>
                    </div>
                    <div class="toggle">
                      <TrashCanIcon class="click-icon-2" @click="deleteAlarm()"/>
                      <ToggleButton
                          :activeAlarm = "() => activeAlarm(alarm.id)"/>
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

          <PopUpDeleteAlarm v-if="deleteAsk"
                            :confirmDelete="() => confirmDelete(id)"
                            :cancelDelete="() => cancelDelete()">
            Are you sure you want to delete {{ alarm.name }}?
          </PopUpDeleteAlarm>

          <PopUpEditAlarm v-if="clickEdit"
                          :confirmEdit="() => confirmEdit(id)"
                          :cancelEdit="() => cancelEdit()"
                          :alarm="alarm"/>

        </div>
    </div>
</template>

<script>
export default {
    props: { alarms: [] },
    data() {
        return {
            itemDays: ["MO", "TU", "WE", "TH", "FR", "SA", "SU"],
            deleteAsk: false,
            clickEdit: false,
        }
    },
    methods: {
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
        deleteAlarm() {
          this.deleteAsk = true;
        },
        confirmDelete(id) {
          let _index = this.alarms.findIndex(obj => obj.id === id);
          this.alarms.splice(_index, 1);

          this.deleteAsk = false;
        },
        cancelDelete() {
          this.deleteAsk = false;
        },
        editAlarm() {
          this.clickEdit = true;
        },
        confirmEdit(id) {
          this.clickEdit = false;
        },
        cancelEdit() {
          this.clickEdit = false;
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