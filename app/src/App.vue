<template>
  <v-app>
    <v-main>
      <v-btn @click="add">Add</v-btn>
      <HelloWorld :tableId="tableId" :headers="headers" v-model:items="items" />
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import { data } from "./data.js"

export default {
  name: 'App',

  components: {
    HelloWorld,
  },

  data: () => ({
    tableId: "1",
    headers: data.headers,
    items: data.items,
  }),
  methods: {
    add() {
      this.items.push({
        name: "Frozen Yogurt",
        calories: 159,
        fat: 6.0,
        carbs: 24,
        protein: 4.0,
        iron: "1%",
      })
      console.log(this.items);
    }
  }
}
</script>
