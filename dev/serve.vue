<script>
import Vue from "vue";
import VNepalidatepicker from "@/v-nepalidatepicker.vue";

export default Vue.extend({
  name: "ServeDev",
  components: {
    VNepalidatepicker
  },
  data() {
    return {
      date: ""
    };
  },
  methods: {
    dateInAD(val) {
      console.log("ad: ", val);
    },
    dateInBS(val) {
      console.log("bs: ", val);
    }
  }
});
</script>

<template>
  <div id="app">
    <v-nepalidatepicker
      v-model="date"
      v-slot="scope"
      @ad="dateInAD"
      @change="dateInBS"
    >
      <input type="text" v-model="scope.formatedValue" @focus="scope.show" />
    </v-nepalidatepicker>
    {{ date }}
  </div>
</template>
