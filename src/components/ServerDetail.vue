<template>
  <div>
    <v-progress-circular indeterminate v-show="$apollo.loading && mcServer == null">
    </v-progress-circular>
    <v-alert  type="error" v-show="!$apollo.loading && mcServer == null">
      Error while loading server data. Reload page to try again.
    </v-alert>
    <div v-if="mcServer">
      {{mcServer.name}}
      {{mcServer.publicAddress}}
    </div>

  </div>
</template>

<script>
import gql from 'graphql-tag';

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
  }),
  apollo: {
    mcServer: {
      query: gql`query mcServer($serverId: String!) {
        mcServer(serverId: $serverId) {
          id
          name
          version
          publicAddress
          status {
            isOnline
            onlinePlayerCount
            maxPlayerCount
            queryTime
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
