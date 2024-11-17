<template>
  <v-layout class="rounded rounded-md">
    <v-main class="d-flex align-center justify-center" style="min-height: 100vh;">
      <FlightAmountEditor :user="user" v-on:update:flights="updateUser" />
    </v-main>
  </v-layout>
</template>

<script lang="ts">
  import FlightAmountEditor from "@/components/FlightAmountEditor.vue";
  export default {
    name: 'FlightEditor',
    components: {FlightAmountEditor},
    data() {
      return {
        user: {
          id: 123,
          name: 'Rick Grimes',
          flights: 1,
          maxFlights: 3
        },
      }
    },
    methods: {
      updateUser(data : {id: number, name: string, flights: number, maxFlights: number}) {
        this.user = {
          ...this.user,
          ...data
        };
      }
    }
  }
</script>
