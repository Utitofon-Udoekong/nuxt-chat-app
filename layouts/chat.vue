<template>
  <v-app :style="{background: $vuetify.theme.themes.dark.background}">
    <v-navigation-drawer v-model="drawer" app mobile-break-point="650" color="#000">
      <v-list subheader>
        <v-subheader>Users in room</v-subheader>

        <v-list-item
          v-for="({ name, id }, index) in users"
          :key="`user-${index}`"
          @click.prevent
          active-class="red--text"
        >
          <v-list-item-content>
            <v-list-item-title v-text="name" />
          </v-list-item-content>
          <v-list-item-icon>
            <v-badge :color="onlineStatus" dot overlap>
              <v-icon :color="id === user.id ? 'primary' : 'grey'">mdi-account-circle-outline</v-icon>
            </v-badge>
          </v-list-item-icon>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app>
      <!-- color="#424242" -->
      <v-app-bar-nav-icon @click="drawer = !drawer" />
      <v-toolbar-title>
        Room
        <v-chip color="grey">{{ user.room }}</v-chip>
        <v-avatar color="red">
          <span>{{user.name.slice(0,2)}}</span>
        </v-avatar>
      </v-toolbar-title>
      <v-spacer />
      <v-btn icon class="mx-1" @click="exit">
        <v-icon>mdi-exit-to-app</v-icon>
      </v-btn>
      <v-btn icon class="mx-1" @click="clearAll">
        <v-icon>mdi-recycle</v-icon>
      </v-btn>
    </v-app-bar>

    <v-content>
      <v-container fluid style="height: 100%;">
        <nuxt />
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import { mapState, mapMutations, mapActions } from "vuex";

export default {
  name: "ChatLayout",
  data: () => ({
    drawer: true,
  }),
  computed: {
    ...mapState(["user", "users"]),
    onlineStatus(){
        switch (navigator.onLine) {
        case true:
          return 'green'        
        case false:
          return 'red'
      
        default:
          return 'red'
      }
    }
  },
  middleware: "auth",
  created() {
    this.joinRoom(this.user);
  },
  methods: {
    ...mapActions(["joinRoom", "leftRoom", "deleteMessage"]),
    exit() {
      this.leftRoom();
      this.$router.push("/?message=leftChat");
    },
    clearAll(){
      this.deleteMessage();
    }
  },
};
</script>
<style lang="scss" scoped>
html {
  overflow-x: hidden;
}
</style>
