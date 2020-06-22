<template>
  <v-container class="fluid">
    <v-row align="center" justify="center">
      <v-card id="dash" :loading="$apollo.loading" style="width: 100%">
        <v-card-title>Tallcraft Servers</v-card-title>
        <v-card-text>
          <v-alert  type="error" v-show="!$apollo.loading && mcServers == null">Error while fetching data. Reload page to try again.</v-alert>
          <ServerList v-if="mcServers" :servers="mcServers"></ServerList>
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
  apollo: {
    mcServers: {
      query: gql`query {
        mcServers{
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
      errorPolicy: 'all',
      // TODO: make poll interval dependent on api refresh rate.
      pollInterval: 5 * 60 * 1000, // Refresh every 5 minutes
    },
  },
};
</script>

<style scoped>
  #dash {
    max-width: 1264px;
  }
</style>
