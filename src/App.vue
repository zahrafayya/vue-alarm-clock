<script setup>
  import Clock from './components/Clock.vue'
  import Alarm from './components/Alarm.vue'
</script>

<template>
  <Clock/>
  <div class="alarm">
    <Alarm/>
  </div>
  
</template>

<style>
.alarm
{
  margin-top: 92px;
}
</style>




