<template>
  <div>
    <div v-show="servers.length === 0">No servers</div>
    <v-expansion-panels multiple>
        <ServerListItem
          v-for="server in servers"
          :key="server.id"
          :server="server"
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
