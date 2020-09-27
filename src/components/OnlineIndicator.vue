<template>
  <span :class="indicatorClass" :title="tooltip"></span>
</template>

<script>
export default {
  name: 'OnlineIndicator',
  props: {
    onlineStatus: {},
    tooltipOnline: {
      type: String,
      default: 'Online',
    },
    tooltipOffline: {
      type: String,
      default: 'Offline',
    },
    tooltipUnknown: {
      type: String,
      default: 'Unknown',
    },
  },
  computed: {
    indicatorClass() {
      return {
        indicator: true,
        online: this.onlineStatus === true,
        offline: this.onlineStatus === false,
      };
    },
    tooltip() {
      if (this.onlineStatus === true) {
        return this.tooltipOnline;
      }
      if (this.onlineStatus === false) {
        return this.tooltipOffline;
      }
      return this.tooltipUnknown;
    },
  },
};
</script>

<style scoped>
  span::before {
    content: ' ';
    display: inline-block;
    width: 7px;
    height: 7px;
    border: 1px solid #000;
    border-radius: 7px;
  }

  .online::before {
    background-color: #94E185;
    border-color: #78D965;
    box-shadow: 0 0 4px 1px #94E185;
  }

  .offline::before {
    background-color: #C9404D;
    border-color: #C42C3B;
    box-shadow: 0 0 4px 1px #C9404D;
  }

</style>
