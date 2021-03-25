<template>
  <v-app>
    <NavigationDrawer />
    <v-main class="grey lighten-5">
      <v-container>
        <router-view></router-view>
        <v-row>
          <v-col cols="12" offset-sm="3">
            <v-container class="mt-5">
              <h2 v-if="$route.name == 'Home'">All for today - {{ hoy }}</h2>
              <h2 v-if="$route.name == 'Personal'">Personal - {{ hoy }}</h2>
              <h2 v-if="$route.name == 'Escuela'">School - {{ hoy }}</h2>
              <h2 v-if="$route.name == 'Trabajo'">Work - {{ hoy }}</h2>

              <v-alert
                id="alerta"
                :class="item.aprobado ? 'cyan accent-2' : 'white'"
                transition="slide-x-transition"
                mode="in-out"
                elevation="3"
                v-for="(item, i) in listaTareas"
                v-show="item.categoria == $route.name || $route.name == 'Home'"
                :key="i"
                class="rounded-xl ma-5"
                max-width="500"
                max-height="165"
              >
                <template>
                  <div class="d-flex justify-end">
                    <v-icon color="black" v-if="item.categoria == 'Personal'"
                      >mdi-account</v-icon
                    >

                    <v-icon color="black" v-if="item.categoria == 'Escuela'"
                      >mdi-book-outline</v-icon
                    >

                    <v-icon color="black" v-if="item.categoria == 'Trabajo'"
                      >mdi-briefcase-outline</v-icon
                    >
                  </div>

                  <v-switch v-model="item.aprobado">
                    <template v-slot:label>
                      <div>
                        <p class="font-weight-black pb-8">{{ item.tarea }}</p>

                        <p
                          class="text-left subtitle-2 ma-2 pb-10 grey--text mt-n12"
                        >
                          {{ item.descripcion }}
                        </p>
                        <p
                          class="text-left subtitle-2 ma-2 font-italic grey--text mt-n12"
                        >
                          {{ item.hora }}
                          <span v-if="item.hora >= '12'">PM</span>
                          <span v-else>AM</span>
                        </p>
                      </div>
                    </template>
                  </v-switch>
                  <div class="d-flex justify-end align-center mt-n12">
                    <h5 class="subtitle-2">Realizada {{ item.aprobado }}</h5>
                    <v-btn icon @click="eliminarTarea(i)"
                      ><v-icon color="red">mdi-delete</v-icon></v-btn
                    >
                  </div>
                </template>
              </v-alert>
              <h2>All for - {{ mañana }}</h2>
              <v-fade-transition>
                <div
                  v-if="$route.path != '/'"
                  class="font-weight-light font-italic grey--text"
                >
                  Ups, there is not more tasks
                </div>
              </v-fade-transition>
            </v-container>
          </v-col>
        </v-row>
        <v-dialog
          class="d-flex justify-center"
          max-width="1000"
          v-model="dialog"
        >
          <v-card class="pr-12 pl-12 pb-12 pt-8" color="white">
            <h1 class="ml-n10 mt-n2 pl-5 pb-5">New Task</h1>
            <h6 class="title black--text">Task name</h6>
            <v-text-field
              solo
              class="rounded-xl"
              height="50"
              @keyup.enter="agregarTarea(), (dialog = false)"
              v-model="tarea"
              label="What are you going to do?"
            ></v-text-field>
            <h6 class="title black--text">Description</h6>
            <v-text-field
              solo
              class="rounded-xl"
              height="50"
              v-model="descripcion"
              label="Descripcion"
              @keyup.enter="agregarTarea(), (dialog = false)"
            ></v-text-field>
            <h6 class="title black--text">Select category</h6>
            <v-select
              solo
              height="50"
              class="rounded-xl"
              color="green"
              label="Category"
              placeholder="select a category"
              dense
              :items="categoria"
              v-model="categoria"
            ></v-select>
            <h6 class="title black--text">Task Complete</h6>
            <v-card
              height="50"
              class="d-flex align-center pl-5 pr-5 pt-2 pb-2 mb-5 rounded-xl"
            >
              <v-switch
                v-model="aprobado"
                :label="`Task complete: ${aprobado.toString()}`"
              ></v-switch>
            </v-card>
            <h6 class="title black--text">Date</h6>
            <v-menu
              ref="menu"
              height="50"
              class="d-flex align-center"
              v-model="menu"
              :close-on-content-click="false"
              transition="slide-y-transition"
              offset-y
              max-width="290px"
              min-width="290px"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-text-field
                  height="50"
                  class="rounded-xl"
                  solo
                  color="green"
                  v-model="horaTarea"
                  label="date"
                  placeholder="Select a date for you task"
                  v-bind="attrs"
                  v-on="on"
                ></v-text-field>
              </template>
              <v-time-picker
                v-model="horaTarea"
                color="green"
                format="ampm"
                ampm-in-title
                @input="menu = false"
              ></v-time-picker>
            </v-menu>

            <v-card-actions class="d-flex justify-end">
              <div>
                <v-btn
                  color="blue"
                  class="text-capitalize"
                  dark
                  rounded
                  x-large
                  @click="(dialog = false), agregarTarea()"
                >
                  Agregar
                </v-btn>
              </div>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-container class="text-right">
          <v-btn fab color="cyan accent-2" bottom @click="dialog = !dialog">
            <v-icon color="white">mdi-plus</v-icon>
          </v-btn>
        </v-container>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import NavigationDrawer from "./components/NavigationDrawer";
const moment = require("moment");
export default {
  name: "App",

  components: {
    NavigationDrawer,
  },
  mounted() {
    this.obtenerLocalStorage();
    this.listaTareas == this.localStorageTareas;
  },
  data() {
    return {
      moment: moment,
      hoy: moment().format("Do MMMM YYYY "),
      mañana: moment().add(1, "days").format("Do MMMM YYYY "),
      aprobado: false,
      menu: false,
      dialog: false,
      tarea: "",
      horaTarea: "",
      listaTareas: [],
      localStorageTareas: [],
      descripcion: "",
      categoria: ["Personal", "Escuela", "Trabajo"],
    };
  },
  methods: {
    async agregarTarea() {
      await this.obtenerLocalStorage();
      this.listaTareas.push({
        tarea: this.tarea,
        descripcion: this.descripcion,
        aprobado: this.aprobado,
        categoria: this.categoria,
        hora: this.horaTarea,
      });

      localStorage.setItem("listaTareas", JSON.stringify(this.listaTareas));
      let listaTareas = JSON.parse(localStorage.getItem("listaTareas"));
      console.log(listaTareas);

      this.tarea = "";
      this.descripcion = "";
      this.aprobado = false;
      this.categoria = ["Personal", "Escuela", "Trabajo"];
      this.horaTarea = "";
    },
    async obtenerLocalStorage() {
      let localStorageTareas = JSON.parse(localStorage.getItem("listaTareas"));

      if (localStorageTareas == null) {
        localStorageTareas = [];
      } else {
        localStorageTareas.forEach((element) => {
          console.log(element);
          this.listaTareas = localStorageTareas;
        });
      }
    },
    eliminarTarea(i) {
      this.listaTareas.splice(i, 1);
    },
  },
};
</script>
