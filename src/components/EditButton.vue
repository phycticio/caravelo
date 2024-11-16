<template>
  <v-dialog max-width="670">
    <template v-slot:activator="{ props: activatorProps }">
      <v-btn
        v-bind="activatorProps"
        color="surface-variant"
        text="Edit Flights"
        variant="outlined"
        prepend-icon="mdi-pencil"
      ></v-btn>
    </template>

    <template v-slot:default="{ isActive }">
      <v-card>
        <v-card-title>
          Edit Flights
        </v-card-title>
        <v-card-subtitle>
          Add or remove flights from the subscriber
        </v-card-subtitle>

        <v-container>
          <v-row>
            <v-col md="4">
              <v-text-field>
                <template v-slot:append>
                  <v-icon>
                    mdi-plus-circle-outline
                  </v-icon>
                </template>
                <template v-slot:prepend>
                  <v-icon>
                    mdi-minus-circle-outline
                  </v-icon>
                </template>
              </v-text-field>
            </v-col>
            <v-col md="8">
              Como estas?
            </v-col>
          </v-row>
        </v-container>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn
            text="Close Dialog"
            @click="isActive.value = false"
          ></v-btn>
        </v-card-actions>
      </v-card>
    </template>
  </v-dialog>
</template>

<script setup lang="ts">
  //
</script>
