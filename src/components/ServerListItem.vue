<template>
  <v-expansion-panel :disabled="hideServerInfo">
    <v-expansion-panel-header class="justify-space-between flex-wrap">
      <OnlineIndicator class="flex-grow-0 pr-4" :online-status="status.isOnline"></OnlineIndicator>
      <h4>{{name}}</h4>
      <v-spacer></v-spacer>
      <v-chip class="flex-grow-0 playerCounter mr-4">
        {{playerCount.current}}/{{playerCount.max}}
      </v-chip>
    </v-expansion-panel-header>
    <v-expansion-panel-content>
      <v-lazy>
      <ServerDetail :server-id="id"></ServerDetail>
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
    id: {
      type: String,
      required: true,
    },
    name: {
      type: String,
      required: true,
    },
    version: {
      type: String,
    },
    publicAddress: {
      type: String,
    },
    status: {},
  },
  computed: {
    hideServerInfo() {
      return this.status?.isOnline === false;
    },
    playerCount() {
      let current = this.status?.onlinePlayerCount;
      let max = this.status?.maxPlayerCount;
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
