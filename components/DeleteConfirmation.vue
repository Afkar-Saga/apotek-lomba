<template>
  <div class="overlay">
    <div class="dialog">
      <div class="dialog-text">Apakah Anda Yakin?</div>
      <div class="dialog-actions">
        <button class="submit cancel" @click="$emit('cancel')">Cancel</button>
        <button class="submit delete" @click="$emit('delete')">Delete</button>
      </div>
    </div>
  </div>
</template>

<script setup>

</script>

<style scoped>
@import url('~/assets/css/main.css');
.overlay {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  background-color: rgba(100, 100, 100, .5);
}
.dialog {
  padding: 20px;
  background-color: white;
  border-radius: 15px;
  text-align: center;
  position: absolute;
  top: 50%;
  left: 50%;
  translate: -50% -50%;
}
.dialog-text {
  font-size: 1.5em;
  font-weight: bold;
  margin-bottom: 20px;
}
.dialog-actions {
  display: flex;
  gap: 20px;
}
.cancel {
  background-color: #fafafa;
  color: black;
  border: 1px solid black;

  &:hover {
    background-color: #e0e0e0;
  }
}
</style>