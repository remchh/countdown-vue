<script setup>
import CountdownHeader from "@/components/CountdownHeader.vue";
import CountdownSegment from "@/components/CountdownSegment.vue";
import { ref, watchEffect } from 'vue'



const secondsEnd = new Date("2024-12-31").getTime()

const time = ref(new Date().getTime())

const timeLeft = ref(secondsEnd - time)

const days = ref(0)
const hours = ref(0)
const minutes = ref(0)
const seconds = ref(0)


// Update countdown values
const updateCountdown = () => {
  timeLeft.value = secondsEnd - time.value
  days.value = Math.floor(timeLeft.value / (1000 * 60 * 60 * 24))
  hours.value = Math.floor((timeLeft.value % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60))
  minutes.value = Math.floor((timeLeft.value % (1000 * 60 * 60)) / (1000 * 60))
  seconds.value = Math.floor((timeLeft.value % (1000 * 60)) / 1000)
}

// Update countdown every second
watchEffect((onInvalidate) => {
  const intervalId = setInterval(() => {
    time.value = new Date().getTime()
    updateCountdown()
  }, 1000)

  onInvalidate(() => {
    clearInterval(intervalId)
  })
})

</script>

<template>
  <div class="app-wrapper">
    <div class="countdown-box">
      <CountdownHeader />
      <main class="flex justify-center">
          <CountdownSegment data-test="days" label="days" :number="days" />
          <CountdownSegment data-test="hours" label="hours" :number="hours" />
          <CountdownSegment data-test="minutes" label="minutes" :number="minutes" />
          <CountdownSegment data-test="seconds" label="seconds" :number="seconds" />
      </main>
    </div>
  </div>
</template>

<style scoped>
.app-wrapper {
  @apply flex items-center justify-center w-full h-full p-10;
}
.countdown-box {
  @apply shadow-md relative bg-white p-5 rounded-lg border-gray-100 border-[1px];
}
body {
  @apply bg-gray-100;
}

.slide-up-enter-active,
.slide-up-leave-active {
  transition: all 0.25s ease-out;
}

.slide-up-enter-from {
  opacity: 0;
  transform: translateY(30px);
}

.slide-up-leave-to {
  opacity: 0;
  transform: translateY(-30px);
}
</style>
