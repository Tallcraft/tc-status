<template>
  <v-expansion-panel :disabled="hideServerInfo">
    <v-expansion-panel-header class="justify-space-between flex-wrap">
      <OnlineIndicator class="flex-grow-0 pr-4" :online-status="server.status.isOnline"></OnlineIndicator>
      <h4>{{server.name}}</h4>
      <v-spacer></v-spacer>
      <v-chip class="flex-grow-0 playerCounter mr-4">
        {{playerCount.current}}/{{playerCount.max}}
      </v-chip>
    </v-expansion-panel-header>
    <v-expansion-panel-content>
      <v-lazy>
      <ServerDetail :mc-server="server"></ServerDetail>
      </v-lazy>
    </v-expansion-panel-content>
  </v-expansion-panel>
</template>

<script>
import OnlineIndicator from './OnlineIndicator.vue';
import ServerDetail from './ServerDetail.vue';

export default {
  name: 'Server',
  components: { ServerDetail, OnlineIndicator },
  props: {
    server: {
      type: Object,
      required: true,
    },
  },
  computed: {
    hideServerInfo() {
      return this.server.status?.isOnline === false;
    },
    playerCount() {
      let current = this.server.status?.onlinePlayerCount;
      let max = this.server.status?.maxPlayerCount;
      if (current == null) {
        current = '-';
      }
      if (max == null) {
        max = '-';
      }
      return {
        current,
        max,
      };
    },
  },
};
</script>

<style scoped>
</style>
