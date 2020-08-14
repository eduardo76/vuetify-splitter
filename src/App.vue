<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-img alt="Vuetify Logo" class="shrink mr-2" contain src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png" transition="scale-transition" width="40"/>
        <v-img alt="Vuetify Name" class="shrink mt-1 hidden-sm-and-down" contain min-width="100" src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png" width="100"/>
      </div>

      <v-spacer></v-spacer>

      <v-btn href="https://github.com/vuetifyjs/vuetify/releases/latest" target="_blank" text>
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>

      <splitter gutter-size="4" min-width="200" v-model="position">

        <template v-slot:panel1>
          <v-card flat>
            <v-tree></v-tree>
            <v-list></v-list>
          </v-card>
        </template>
        
        <template v-slot:panel2="{ right }">
          <v-container id="blah">
            <h1>Panel2 - {{right}}</h1>
            <h1>Panel2 - {{position}}</h1>
          </v-container>
        </template>

      </splitter>
    </v-main>
  </v-app>
</template>

<script>

import Splitter from "@/components/Splitter.vue";
import VTree from "@/components/VTree.vue";
import VList from "@/components/VList.vue";

export default {
  name: 'App',

  data() {
    return {
      position: 400
    }
  },

  components: {
    Splitter,
    VTree,
    VList
  },

  mounted () {
    let position = localStorage.getItem('SplitterPosition');
    if (position) {
      this.position = position;
    }
  },

  watch: {
    position(val, oldval) {
      localStorage.setItem('SplitterPosition', val)
    }
  },

};
</script>

<style scoped>

</style>