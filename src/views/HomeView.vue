<script setup lang="ts">
</script>

<template>
  <main>
    <h2 class="main-header">IDEN Calculator</h2>
    <br>
    <p>Downlink Frequency (MHz): <input v-model="downlink" v-on:input="updateId"/></p>
    <p>Base Frequency (MHz): <input v-model="base" v-on:input="updateId"/></p>
    <p>Spacing (KHz): <input v-model="spacing" v-on:input="updateId"/></p>
    <p>Offset (MHz): <input v-model="offset" v-on:input="updateId"/></p>
    <div class="alert alert-danger" v-show="invalid">Woah there! Your channel number is invalid! <br> Reason: {{ validation_message }}</div>
    <p>Channel Number (dec): <input v-model="id" v-on:input="updateDownlink"/> (0x{{ id.toString(16) }})</p>
    <p>Uplink Frequency (MHz): <b>{{ uplink }}MHz</b></p>
    <p class="footer-text">iden-calc-web V0.2 copyright &copy; 2023, 2025 Natalie Moore, Connor Lovell and the <a href="https://github.com/dvmproject">DVMProject</a> team.</p>
  </main>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
    name: 'HomeView',
    data() {
        return {
            id: 0 as number,
            downlink: 0 as number,
            uplink: 0 as number,
            base: 450 as number,
            spacing: 6.25 as number,
            offset: 5 as number,
            invalid: 0 as number,
            validation_message: "" as string,
        }
    },
    methods: {
        async updateId() {
          var downlinkHz = this.downlink * 1000000
          var baseHz = this.base * 1000000
          var spacingHz = this.spacing * 1000
          var new_id = (downlinkHz - baseHz) / spacingHz

          if (new_id % 1 > 0) {
            this.invalid = 1;
            this.id = 0;
            this.validation_message = "Fractional channel number: must be a whole number"
          } else if (new_id > 4095) {
            this.invalid = 1;
            this.id = 0;
            this.validation_message = "Too Large: channel number must be less than 4095 (0xFFF)"
          } else if (new_id < 0) {
            this.invalid = 1;
            this.id = 0;
            this.validation_message = "Too Small: channel number must be greater than zero"
          } else {
            this.invalid = 0;
            this.id = new_id;
          }

          this.uplink = (this.downlink * 1 + this.offset * 1)
        },
        async updateDownlink() {
          var downlinkHz = this.downlink * 1000000
          var baseHz = this.base * 1000000
          var spacingHz = this.spacing * 1000
          this.downlink = (baseHz + (this.id * spacingHz)) / 1000000
          this.uplink = (this.downlink * 1 + this.offset * 1)
        }
    },
    async mounted() {
        //
    }
})
</script>