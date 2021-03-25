<template>
  <v-app>
    <NavigationDrawer />
    <v-main class="grey lighten-5">
      <v-container>
        <router-view></router-view>
        <v-row>
          <v-col cols="12" offset-sm="3">
            <v-container>
              <v-alert
                :class="item.aprobado ? 'cyan accent-2' : 'white'"
                transition="slide-x-transition"
                mode="in-out"
                elevation="3"
                v-for="(item, i) in listaTareas"
                v-show="item.categoria == $route.name || $route.name == 'Home'"
                :key="i"
                class="rounded-xl ma-5 transition-swing"
                max-width="500"
              >
                <template>
                  <v-icon color="black" v-if="item.categoria == 'Personal'"
                    >mdi-account</v-icon
                  >

                  <v-icon color="black" v-if="item.categoria == 'Escuela'"
                    >mdi-book-outline</v-icon
                  >

                  <v-icon color="black" v-if="item.categoria == 'Trabajo'"
                    >mdi-briefcase-outline</v-icon
                  >

                  <h3 class="text-right font-weight-medium">
                    {{ i }} - {{ item.tarea }}
                  </h3>
                  <v-switch
                    :label="`Realizado: ${item.aprobado}`"
                    v-model="item.aprobado"
                  >
                  </v-switch>
                  <p class="text-right subtitle-2 grey--text mt-n12">
                    {{ item.descripcion }}
                  </p>
                </template>
                <div class="d-flex justify-space-between">
                  <v-btn icon @click="eliminarTarea(i)"
                    ><v-icon color="red">mdi-delete</v-icon></v-btn
                  >
                </div>
              </v-alert>
            </v-container>
          </v-col>
        </v-row>
        <v-dialog v-model="dialog" max-width="500px">
          <v-card class="pa-5">
            <v-text-field
              @keyup.enter="agregarTarea(), (dialog = false)"
              v-model="tarea"
              label="Titulo"
            ></v-text-field>
            <v-text-field
              v-model="descripcion"
              label="Descripcion"
              @keyup.enter="agregarTarea(), (dialog = false)"
            ></v-text-field>
            <v-select
              outlined
              color="green"
              label="Categoria"
              placeholder="Selecciona una categoria"
              dense
              :items="categoria"
              v-model="categoria"
            ></v-select>
            <v-switch
              v-model="aprobado"
              :label="`Tarea completada: ${aprobado.toString()}`"
            ></v-switch>
            <v-card-actions>
              <v-spacer></v-spacer>

              <v-btn
                text
                color="primary"
                @click="(dialog = false), agregarTarea()"
              >
                Agregar
              </v-btn>
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

export default {
  name: "App",

  components: {
    NavigationDrawer,
  },

  data() {
    return {
      aprobado: false,
      dialog: false,
      tarea: "",
      listaTareas: [],
      descripcion: "",
      categoria: ["Personal", "Escuela", "Trabajo"],
    };
  },
  methods: {
    agregarTarea() {
      this.listaTareas.push({
        tarea: this.tarea,
        descripcion: this.descripcion,
        aprobado: this.aprobado,
        categoria: this.categoria,
      });
      console.log(this.listaTareas);
      this.tarea = "";
      this.descripcion = "";
      this.aprobado = false;
      this.categoria = ["Personal", "Escuela", "Trabajo"];
    },
    eliminarTarea(i) {
      this.listaTareas.splice(i, 1);
    },
  },
};
</script>
