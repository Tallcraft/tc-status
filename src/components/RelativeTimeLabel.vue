<template>
  <span>{{label}}</span>
</template>

<script>
import moment from 'moment';

export default {
  name: 'RelativeTimeLabel',
  props: {
    timestamp: {
      description: 'Timestamp to show label for. May be UNIX string or Date object.',
      validator(value) {
        return typeof value === 'number'
          || typeof value === 'string'
          || value instanceof Date;
      },
    },
    updateInterval: {
      description: 'How often to update the label in seconds',
      type: Number,
      default: 5,
    },
  },
  data: () => ({
    intervalID: null,
    currentTime: new Date(),
  }),
  mounted() {
    this.setTimer();
  },
  beforeDestroy() {
    if (this.intervalID != null) {
      clearInterval(this.intervalId);
    }
  },
  methods: {
    setTimer() {
      if (this.intervalID != null) {
        clearInterval(this.intervalID);
      }
      this.intervalID = setInterval(() => {
        this.currentTime = new Date();
      }, this.updateInterval * 1000);

      // Run first update manually
      this.currentTime = new Date();
    },
  },
  watch: {
    // the interval setting changed, reset our timer
    updateInterval() {
      this.setTimer();
    },
  },
  computed: {
    timestampParsed() {
      if (this.timestamp instanceof Date) {
        return this.timestamp;
      }
      try {
        let tsRaw = this.timestamp;
        if (typeof tsRaw === 'string') {
          tsRaw = Number.parseInt(tsRaw, 10);
        }
        return new Date(tsRaw);
      } catch (error) {
        return null;
      }
    },
    label() {
      return moment(this.timestampParsed).from(this.currentTime);
    },
  },
};
</script>

<style scoped>

</style>
