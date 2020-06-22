<template>
  <div>
    <p>Is online: {{isOnline}}</p>
    <p>Player count: {{onlinePlayerCount}}/{{maxPlayerCount}}</p>
    <p>Queried {{queryTimeRelative}}</p>
  </div>
</template>

<script>
import moment from 'moment';

export default {
  name: 'ServerStatus',
  props: {
    isOnline: {
      type: Boolean,
      required: true,
    },
    onlinePlayerCount: {
      type: Number,
    },
    maxPlayerCount: {
      type: Number,
    },
    queryTime: {
      type: String,
    },
  },
  computed: {
    queryTimeParsed() {
      if (!this.queryTime) {
        return null;
      }
      try {
        return new Date(Number.parseInt(this.queryTime, 10));
      } catch (error) {
        return null;
      }
    },
    queryTimeRelative() {
      const parsedTime = this.queryTimeParsed;
      if (!parsedTime) {
        return null;
      }
      return moment(parsedTime).fromNow();
    },
  },
};
</script>

<style scoped>

</style>
