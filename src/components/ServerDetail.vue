<template>
  <div>

    <v-progress-circular indeterminate v-show="$apollo.loading && mcServer == null">
    </v-progress-circular>
    <v-alert  type="error" v-show="!$apollo.loading && mcServer == null">
      Error while loading server data. Reload page to try again.
    </v-alert>
    <div v-if="mcServer">
      <v-list>
        <v-list-item-group>
          <v-list-item @click="copyServerAddress">
            <v-list-item-icon><v-icon>mdi-controller-classic-outline</v-icon></v-list-item-icon>
            <v-list-item-content>
              <b>Address</b>
              {{mcServer.publicAddress}}
            </v-list-item-content>
          </v-list-item>
          <v-list-item>
            <v-list-item-icon><v-icon>mdi-information-outline</v-icon></v-list-item-icon>
            <v-list-item-content>
              <b>Version</b>
              {{mcServer.version}}
            </v-list-item-content>
          </v-list-item>
          <!-- TODO: remove serverId exception once API bug is fixed -->
          <v-list-item v-if="serverId !== 'global'">
            <v-list-item-icon><v-icon>mdi-account-multiple-outline</v-icon></v-list-item-icon>
            <v-list-item-content>
              <b>Players</b>
              {{onlinePlayerList}}
            </v-list-item-content>
          </v-list-item>
          <v-list-item>
            <v-list-item-icon><v-icon>mdi-clock-outline</v-icon></v-list-item-icon>
            <v-list-item-content>
              <b>Last updated</b>
              {{updatedLabel}}
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </div>

  </div>
</template>

<script>
import gql from 'graphql-tag';
import moment from 'moment';

export default {
  name: 'ServerDetail',
  props: {
    serverId: {
      type: String,
      required: true,
    },
  },
  data: () => ({
    mcServer: null,
    currentTime: new Date(),
  }),
  beforeMount() {
    setInterval(() => {
      this.currentTime = new Date();
    }, 5000);
  },
  methods: {
    copyServerAddress() {
      if (!navigator.clipboard?.writeText) {
        console.warn('Cannot copy server address to clipboard, browser does not support it.');
        return;
      }
      navigator.clipboard.writeText(this.mcServer.publicAddress);
    },
  },
  computed: {
    updatedLabel() {
      if (!this.queryTime) {
        return 'Loading';
      }
      return moment(this.queryTime).from(this.currentTime);
    },
    queryTime() {
      if (!this.mcServer?.status?.queryTime) {
        return null;
      }
      try {
        return new Date(Number.parseInt(this.mcServer.status.queryTime, 10));
      } catch (error) {
        return null;
      }
    },
    // TODO: We should return null and hide the player counter if the onlinePlayers field is null.
    onlinePlayerList() {
      if (!this.mcServer?.status?.onlinePlayers?.length) {
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
  apollo: {
    mcServer: {
      query: gql`query mcServer($serverId: String!) {
        mcServer(serverId: $serverId) {
          version
          publicAddress
          status {
            isOnline
            onlinePlayerCount
            maxPlayerCount
            queryTime
            onlinePlayers {
                name,
            }
          }
        }
    }`,
      variables() {
        return {
          serverId: this.serverId,
        };
      },
      errorPolicy: 'all',
      // TODO: make poll interval dependent on api refresh rate.
      pollInterval: 5 * 60 * 1000, // Refresh every 5 minutes
    },
  },
};
</script>

<style scoped>

</style>
