<template>
  <div>
    <v-list>
      <v-list-item-group>
        <v-list-item @click="copyServerAddress" title="Click to copy">
          <v-list-item-icon>
            <v-icon>mdi-controller-classic-outline</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <b>Address</b>
            {{mcServer.address}}
          </v-list-item-content>
        </v-list-item>
        <v-list-item>
          <v-list-item-icon>
            <v-icon>mdi-information-outline</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <b>Version</b>
            {{mcServer.version}}
          </v-list-item-content>
        </v-list-item>
        <!-- TODO: remove serverId exception once API bug is fixed -->
        <v-list-item v-if="mcServer.id !== 'global' && onlinePlayerList">
          <v-list-item-icon>
            <v-icon>mdi-account-multiple-outline</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <b>Players</b>
            {{onlinePlayerList}}
          </v-list-item-content>
        </v-list-item>
        <v-list-item v-if="showLastUpdatedLabel">
          <v-list-item-icon>
            <v-icon>mdi-clock-outline</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <b>Last updated</b>
            <RelativeTimeLabel
              :timestamp="this.mcServer.status.queryTime"
            ></RelativeTimeLabel>
          </v-list-item-content>
        </v-list-item>
      </v-list-item-group>
    </v-list>
  </div>
</template>

<script>
import RelativeTimeLabel from './RelativeTimeLabel.vue';

export default {
  name: 'ServerDetail',
  components: { RelativeTimeLabel },
  props: {
    mcServer: {
      type: Object,
      required: true,
    },
  },
  methods: {
    copyServerAddress() {
      if (!navigator.clipboard?.writeText) {
        console.warn('Cannot copy server address to clipboard, browser does not support it.');
        return;
      }
      navigator.clipboard.writeText(this.mcServer.address);
    },
  },
  computed: {
    showLastUpdatedLabel() {
      return this.mcServer?.status?.queryTime;
    },
    // TODO: We should return null and hide the player counter if the onlinePlayers field is null.
    onlinePlayerList() {
      if (!this.mcServer?.status.isOnline) {
        return null;
      }
      if (!this.mcServer.status.onlinePlayers?.length) {
        return 'No online players';
      }
      return this.mcServer.status.onlinePlayers.reduce((acc, player) => {
        if (!acc) {
          return player.name;
        }
        return `${acc}, ${player.name}`;
      }, '');
    },
  },
};
</script>

<style scoped>

</style>
