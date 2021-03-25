<template>
  <div>
    <v-app-bar
      color="white"
      elevation="3"
      :clipped-left="clipped"
      :clipped-right="clipped"
      app
    >
      <v-app-bar-nav-icon
        @click.stop="
          (drawer = !drawer), (rightDrawer = true), (miniVariant = !miniVariant)
        "
      />

      <h4 class="title font-weight-black ma-5" v-text="title" />
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
            <v-icon class="ml-5" right color="blue">{{ item.icon }}</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-navigation-drawer
      width="310"
      color="rgba(0,0,0,0)"
      v-model="rightDrawer"
      :right="right"
      :mini-variant="miniVariant"
      :clipped="clipped"
      app
      fixed
    >
      <v-list-item-title class="mt-4 ma-3 title font-weight-black">
        <v-layout align-center>
          <v-list>
            <v-btn
              :disabled="miniVariant == false"
              class="mr-12"
              icon
              @click.stop="miniVariant = !miniVariant"
            >
              <v-icon>mdi-calendar-blank</v-icon>
            </v-btn>
            Calendar
            <v-scale-transition>
              <v-btn
                v-if="miniVariant == false"
                small
                color="info"
                class="ml-16"
                fab
                depressed
                @click.stop="miniVariant = !miniVariant"
                ><v-icon>mdi-arrow-collapse</v-icon></v-btn
              >
            </v-scale-transition>
          </v-list>
        </v-layout>
      </v-list-item-title>
      <v-row justify="center">
        <v-slide-x-transition>
          <v-card
            color="rgba(0,0,0,0)"
            flat
            v-if="miniVariant == false"
            max-width="300"
            class="rounded-xl my-8"
          >
            <v-date-picker v-model="picker"></v-date-picker>
          </v-card>
        </v-slide-x-transition>
      </v-row>
    </v-navigation-drawer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      clipped: true,
      picker: null,
      mini: true,
      drawer: false,
      fixed: false,
      avatar:
        "https://i.pinimg.com/564x/5f/ea/cc/5feacc824b1fc77c643070430b6762c0.jpg",
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
          icon: "mdi-briefcase-outline",
          title: "Work",
          path: "/work",
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