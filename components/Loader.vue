<template>
  <div class="loader">
    <div class="dots"></div>
    <div class="dots"></div>
    <div class="dots"></div>
  </div>
</template>

<script setup>

</script>

<style scoped>
.loader {
  width: 50px;
  height: 20px;
  display: flex;
  justify-content: space-between;
}

.dots {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background-color: #777;
  animation: pulse 1.5s infinite ease-in-out;

  &:nth-of-type(1) {
    animation-delay: 0s;
    background-color: #6efd61;
  }
  &:nth-of-type(2) {
    animation-delay: 0.5s;
    background-color: #e7fc71;
  }
  &:nth-of-type(3) {
    animation-delay: 1s;
    background-color: #fd6161;
  }
}

@keyframes pulse {
  0%, 100% {
    scale: .6;
    opacity: .6;
  }
  50% {
    scale: 1;
    opacity: 1;
  }
}
</style>