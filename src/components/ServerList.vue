<template>
  <div>
    <div v-show="servers.length === 0">No servers</div>
    <v-expansion-panels multiple>
        <ServerListItem
          v-for="server in servers"
          :key="server.id"
          :id="server.id"
          :name="server.name"
          :version="server.version"
          :public-address="server.publicAddress"
          :status="server.status"
        >
        </ServerListItem>
    </v-expansion-panels>
  </div>
</template>

<script>
import ServerListItem from './ServerListItem.vue';

export default {
  name: 'ServerList',
  components: { ServerListItem },
  props: {
    servers: {},
  },
};
</script>

<style scoped>
</style>
