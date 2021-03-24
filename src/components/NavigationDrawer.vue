<template>
  <div>
    <v-app-bar
      color="white"
      :clipped-left="clipped"
      :clipped-right="clipped"
      app
    >
      <v-app-bar-nav-icon
        @click.stop="
          (drawer = !drawer), (rightDrawer = true), (miniVariant = !miniVariant)
        "
      />

      <h4 v-text="title" />
      <v-spacer></v-spacer>
      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <v-btn icon>
        <v-badge content="3" bordered color="red" overlap>
          <v-icon>mdi-bell-outline</v-icon>
        </v-badge>
      </v-btn>
      <v-hover v-slot:default="{ hover }" open-delay="0">
        <v-btn depressed icon>
          <v-avatar :class="`elevation-${hover ? 7 : 7}`" size="37"
            ><v-img :src="avatar"></v-img
          ></v-avatar>
        </v-btn>
      </v-hover>
    </v-app-bar>
    <v-navigation-drawer
      width="220"
      floating
      v-model="drawer"
      :clipped="clipped"
      fixed
      app
    >
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="mt-4 title font-weight-black">
            <v-layout align-center> Collections </v-layout>
          </v-list-item-title>
        </v-list-item-content>
      </v-list-item>

      <v-list class="mt-2 mb-2">
        <v-list-item
          active-class="blue lighten-5 "
          v-for="item in items"
          :key="item.title"
          link
          :to="item.path"
          exact
          class="font-weight-black"
        >
          <v-list-item-icon>
            <v-icon class="ml-5" right>{{ item.icon }}</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <div class="subtitle-2 grey--text mx-4">insights</div>
      <v-list dense nav class="mx-n1">
        <v-list-item link>
          <v-list-item-icon>
            <v-icon>mdi-mail</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>Messages</v-list-item-title>
          </v-list-item-content>

          <v-list-item-avatar color="red" size="20" class="white--text">
            1
          </v-list-item-avatar>
        </v-list-item>
        <v-list-item link>
          <v-list-item-icon>
            <v-icon>mdi-bell</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>
              Notifications
            </v-list-item-title></v-list-item-content
          >

          <v-list-item-avatar size="20" color="deep-purple" class="white--text">
            2
          </v-list-item-avatar>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-navigation-drawer
      color="rgba(0,0,0,0)"
      v-model="rightDrawer"
      :right="right"
      :mini-variant="miniVariant"
      :clipped="clipped"
      app
      fixed
    >
      <v-list dense nav class="mx-n1">
        <v-list-item
          @click.stop="miniVariant = !miniVariant"
          v-for="item in items2"
          :key="item.title"
          link
          :to="item.path"
          exact
        >
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <div class="subtitle-2 grey--text mx-4">insights</div>
    </v-navigation-drawer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      clipped: true,
      mini: true,
      drawer: false,
      fixed: false,
      avatar:
        "https://i.pinimg.com/564x/9a/fe/20/9afe20f84de3f9dbeb4b7109c319c79e.jpg",
      items: [
        {
          icon: "mdi-view-dashboard",
          title: "All tasks",
          path: "/",
        },
        {
          icon: "mdi-account",
          title: "Personal",
          path: "/personal",
        },
        {
          icon: "mdi-book",
          title: "School",
          path: "/school",
        },
        {
          icon: "mdi-rotate-right-variant",
          title: "Work",
          path: "/work",
        },
      ],
      items2: [
        {
          icon: "mdi-clock",
          title: "Home",
          path: "/",
        },
      ],
      miniVariant: true,
      right: true,
      rightDrawer: true,
      title: "My Task list",
    };
  },
};
</script>