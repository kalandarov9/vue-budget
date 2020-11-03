<template>
  <div class="total-value" :style="totalClasses">Balance {{ total }}</div>
</template>

<script>
export default {
  name: "TotalBalance",
  data() {
    return {
      color: "green"
    };
  },

  props: {
    total: {
      type: Number,
      default: 0
    },
    colorbalance: {
      type: String
    }
  },

  computed: {
    totalClasses: function() {
      return {
        color: this.colorbalance
      };
    }
  }
};
</script>

<style scoped>
.total-value {
  font-size: 26px;
  text-transform: uppercase;
  text-align: center;
  padding: 20px;
}
</style>
