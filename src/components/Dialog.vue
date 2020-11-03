<template>
  <el-dialog title="Warning" :visible.sync="visibleDialog" width="30%" center>
    <span>Are you sure you want to remove this item?</span>
    <span
      ><p>Comment: {{ message.comment }}</p>
      <p>Value: {{ message.value }}</p>
    </span>
    <span slot="footer" class="dialog-footer">
      <el-button @click="close">Cancel</el-button>
      <el-button type="primary" @click="confirm">Yes</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  props: {
    message: {
      type: Object,
      default: () => ({})
    }
  },

  data: () => ({
    visibleDialog: false
  }),

  methods: {
    close() {
      this.visibleDialog = false;
    },
    confirm() {
      this.$emit("confirm-answer", true);
      this.close();
    },
    open() {
      this.visibleDialog = true;
    }
  }
};
</script>

<style scoped></style>
