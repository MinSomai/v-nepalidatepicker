<script>
import Vue from "vue";
import VNepalidatepicker from "@/v-nepalidatepicker.vue";

export default Vue.extend({
  name: "ServeDev",
  components: {
    VNepalidatepicker,
  },
});
</script>

<template>
  <div id="app">
    <v-nepalidatepicker v-slot="scope">
      <input type="text" v-model="scope.formatedValue" @focus="scope.show" />
    </v-nepalidatepicker>
  </div>
</template>
