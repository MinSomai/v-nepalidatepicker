<script>
import Vue from "vue";
import VNepalidatepicker from "@/v-nepalidatepicker.vue";

export default Vue.extend({
  name: "ServeDev",
  components: {
    VNepalidatepicker,
  },
  methods: {
    dateInAD(val) {
      console.log(val);
    },
  },
});
</script>

<template>
  <div id="app">
    <v-nepalidatepicker v-slot="scope" @ad="dateInAD">
      <input type="text" v-model="scope.formatedValue" @focus="scope.show" />
    </v-nepalidatepicker>
  </div>
</template>
