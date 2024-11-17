<template>
  <v-dialog max-width="740" closable="true">
    <template v-slot:activator="{ props: activatorProps }">
      <v-btn
        v-bind="activatorProps"
        color="surface-variant"
        :text="`Edit Flights for ${this.user.name} (${this.user.flights})`"
        variant="outlined"
        prepend-icon="mdi-pencil"
      >
      </v-btn>
    </template>

    <template v-slot:default="{ isActive }">
      <v-card class="pa-4">
        <v-card-title>
          Edit Flights for {{ user.name }}
        </v-card-title>
        <v-card-subtitle>
          Add or remove flights from the subscriber
        </v-card-subtitle>
        <v-icon
          @click="isActive.value = false;resetUserQuota();"
          class="cursor-pointer position-absolute top-0 right-0 mt-4 mr-4">
          mdi-close
        </v-icon>

        <v-container class="mt-4">
          <v-row>
            <v-col md="5" class="bg-grey-lighten-3 d-flex align-center justify-center flex-column">
              <h3 class="text-h5 mb-4">
                Flights Left
              </h3>
              <v-text-field :model-value="flights"
                            type="number"
                            variant="solo"
                            class="w-100 app--input_text"
                            readonly>
                <template v-slot:append>
                  <v-icon
                    :disabled="this.flights === this.user.maxFlights"
                    @click="changeQuota()"
                    class="cursor-pointer">
                    mdi-plus-circle-outline
                  </v-icon>
                </template>
                <template v-slot:prepend>
                  <v-icon
                    :disabled="this.flights === 0"
                    @click="changeQuota(false)"
                    class="cursor-pointer">
                    mdi-minus-circle-outline
                  </v-icon>
                </template>
              </v-text-field>
            </v-col>
            <v-col md="7">
              <v-select
                @change="(val : string) => { this.reason = val; }"
                label="What is the motive?"
                :items="reasons"
                v-model="reason"
                variant="outlined"
              ></v-select>
              <v-alert
                v-show="alert.show"
                :type="alert.type">
                <template v-slot:text>
                  <div v-html="alert.text"></div>
                </template>
              </v-alert>
            </v-col>
          </v-row>
        </v-container>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn
            class="bg-red"
            :disabled="!reason || !this.reasons"
            text="Save Changes"
            @click="saveNewQuota()"
          ></v-btn>
        </v-card-actions>
      </v-card>
    </template>
  </v-dialog>
</template>

<script lang="ts">
export default {
  name: 'FlightAmountEditor',
  props: {
    user: {
      name: String,
      id: Number,
      flights: Number,
      maxFlights: Number
    },
  },
  emits: ['update:flights'],
  data() {
    return {
      initialFlightsChanged: false,
      flights: 0,
      initialFlights: 0,
      reasons: [],
      reason: '',
      newQuota: {
        id: null,
        flights: 0,
        reason: ''
      },
      alert: {
        show: false,
        type: 'info',
        text: ''
      }
    }
  },
  mounted() {
    this.flights = this.user.flights;
    this.initialFlights = this.user.flights;
  },
  methods: {
    changeQuota(add: boolean = true) {
      this.flights = add ? this.flights + 1 : this.flights - 1;
      this.flights = this.flights < 0 ? 0 : this.flights;
      this.flights = this.flights === this.user.maxFlights ? this.user.maxFlights : this.flights;
      this.initialFlightsChanged = this.initialFlights !== this.flights;
      this.reasons = this.initialFlightsChanged ? this._getReasons(
        this.flights > this.initialFlights
      ) : [];
      this.reason = this.initialFlightsChanged ? this.reason : '';
    },
    async saveNewQuota() {
      this.newQuota = {
        id: this.user.id,
        flights: this.flights,
        reason: this.reason,
      }

      const url = 'https://httpstat.us/random/200,201,400,500,200,201,200,201';

      const response = await fetch(url)
      try {
        if(!response.ok) {
          throw new Error(response.statusText);
        }

        this.alert.show = true;
        this.alert.text = [
          `${this.user.name}'s quota has been changed to <strong>${this.flights}</strong>`,
          `<br><strong>Reason</strong>: ${this.reason}`
        ].join('');
        this.alert.type = 'success';
        this.initialFlights = this.flights;
        this.reason = '';
        this.reasons = [];
        this.updateUser();
      } catch(e) {
        this.alert.show = true;
        this.alert.text = `${response.status}: ${e.message}`;
        this.alert.type = 'error';
      }
    },
    resetUserQuota() {
      this.flights = this.user.flights;
      this.reason = '';
      this.reasons = [];
    },
    updateUser() {
      this.$emit('update:flights', {flights: this.flights})
    },
    _getReasons(add: boolean | null): string[] {
      return add ?
        [
          'Subscriber canceled flight',
          'Airline canceled flight',
          'Customer compensation',
          'Other'
        ] :
        [
          'Flight not redeposited after a flight cancellation',
          'Subscriber had log in or password issues',
          'Subscriber had issues when booking',
          'Subscription has not renewed correctly',
          'Other'
        ];
    }
  }
}
</script>
<style>
  .v-input__control {
    display: flex;
    align-items: center;
  }
  .v-field {
    height: 3.5rem;
  }
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  input[type=number] {
    -moz-appearance:textfield; /* Firefox */
  }
</style>
