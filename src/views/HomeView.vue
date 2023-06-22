<script setup lang="ts">
</script>

<template>
  <main>
    <h3 class="main-header">IDEN Calculator</h3>
    <p>
      Downlink Frequency (MHz): <input v-model="downlink" v-on:input="updateId"/><br/>
      Base Frequency (MHz): <input v-model="base" v-on:input="updateId"/><br/>
      Spacing (KHz): <input v-model="spacing" v-on:input="updateId"/><br/>
      Offset (MHz): <input v-model="offset" v-on:input="updateId"/><br/>
      Channel ID (dec): <input v-model="id" v-on:input="updateDownlink"/> (0x{{ id.toString(16) }})<br/>
      Uplink Frequency (MHz): {{ uplink }}MHz
    </p>
    <p class="footer-text">iden-calc-web V0.2 copyright &copy; 2023 Natalie Moore, Connor Lovell and the <a href="https://github.com/dvmproject">DVMProject</a> team.</p>
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
        }
    },
    methods: {
        async updateId() {
          var downlinkHz = this.downlink * 1000000
          var baseHz = this.base * 1000000
          var spacingHz = this.spacing * 1000
          this.id = (downlinkHz - baseHz) / spacingHz
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