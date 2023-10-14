<template>
  <div>
    <div v-if="secondsRemaining > 0">
      <div>
        <span>{{ h1 }}</span>
        <span>{{ h2 }}</span>
        <span>:</span>
        <span>{{ m1 }}</span>
        <span>{{ m2 }}</span>
        <span>:</span>
        <span>{{ s1 }}</span>
        <span>{{ s2 }}</span>
      </div>
    </div>
    <p v-else>Countdown has ended!</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      day: 0,
      isWeekend: false,
      endHour: 0,
      secondsRemaining: 0,
      h1: 0,
      h2: 0,
      m1: 0,
      m2: 0,
      s1: 0,
      s2: 0,
    };
  },
  computed: {
    formattedTime() {
      return (
        `${this.h1}${this.h2}:${this.m1}${this.m2}:${this.s1}${this.s2}`
      );
    },
  },
  mounted() {
    this.initExpiration();
    this.startCountdown();
  },
  methods: {
    initExpiration() {
      const date = new Date();
      this.day = date.getDay();
      this.isWeekend = this.day > 5;
      this.endHour = this.isWeekend ? 14 : 24;
    },
    updateDigitValues() {
      const hours = Math.floor(this.secondsRemaining / 3600);
      const minutes = Math.floor((this.secondsRemaining % 3600) / 60);
      const seconds = this.secondsRemaining % 60;

      this.h1 = Math.floor(hours / 10);
      this.h2 = hours % 10;
      this.m1 = Math.floor(minutes / 10);
      this.m2 = minutes % 10;
      this.s1 = Math.floor(seconds / 10);
      this.s2 = seconds % 10;
    },
    startCountdown() {
      const updateInterval = 100; // Update every 100ms
      const targetTime = new Date();

      // Calculate the target time for the countdown
      targetTime.setHours(this.endHour, 0, 0, 0);

      // Create an interval to update the countdown every 100ms
      const countdownInterval = setInterval(() => {
        const now = new Date();
        this.secondsRemaining = Math.max(Math.floor((targetTime - now) / 1000), 0);

        if (this.secondsRemaining <= 0) {
          clearInterval(countdownInterval); // Stop the interval when the countdown ends
        }

        this.updateDigitValues(); // Update digit values on every interval
      }, updateInterval);
    },
  },
};
</script>
