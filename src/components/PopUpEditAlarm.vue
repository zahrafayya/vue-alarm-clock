<template>
  <div class="popup">
    <div class="popup-inner">
      <div class="popup-text">

      </div>
      <div>
        <button class="popup-close-1" @click="confirmEdit()">
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
  props: ['confirmEdit', 'cancelEdit']
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