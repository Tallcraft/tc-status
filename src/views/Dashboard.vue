<template>
  <v-container class="fluid">
    <v-row align="center" justify="center">
      <v-card id="dash" :loading="$apollo.loading" style="width: 100%">
        <v-card-title>Tallcraft Servers</v-card-title>
        <v-card-text>
          <v-alert type="error" v-show="!$apollo.loading && mcServers == null">Error while fetching
            data. Reload page to try again.
          </v-alert>
          <ServerList v-if="mcServers" :servers="mcServers"></ServerList>
          <footer class="mt-3">
            <v-icon dense class="mx-2" @click="$apollo.queries.mcServers.refetch()">
              mdi-refresh
            </v-icon>
            Live data - Updating every {{pollInterval / 1000}} seconds.
          </footer>
        </v-card-text>
      </v-card>
    </v-row>
  </v-container>
</template>

<script>
import gql from 'graphql-tag';
import ServerList from '../components/ServerList.vue';

export default {
  name: 'Dashboard',
  components: { ServerList },
  data: () => ({
    mcServers: null,
  }),
  computed: {
    pollInterval() {
      if (!this.mcServers) {
        return 60 * 1000;
      }
      // Adapt poll interval according to smallest status update rate.
      return this.mcServers.reduce((minInterval, server) => {
        if (minInterval === null || server.statusPollInterval < minInterval) {
          return server.statusPollInterval;
        }
        return minInterval;
      }, null) * 1000;
    },
  },
  apollo: {
    mcServers: {
      query: gql`query {
        mcServers {
          id
          name
          address
          version
          statusPollInterval
          status {
            isOnline
            onlinePlayerCount
            maxPlayerCount
            queryTime
            onlinePlayers {
              name
            }
          }
        }
      }
      `,
      errorPolicy: 'all',
      pollInterval() {
        return this.pollInterval;
      },
    },
  },
};
</script>

<style scoped>
  #dash {
    max-width: 1264px;
  }
</style>
