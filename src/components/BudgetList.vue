<template>
  <div class="budget-list-wrap">
    <ElCard :header="header">
      <ElButton
        type="success"
        icon="el-icon-s-data"
        circle
        @click="sort(true)"
      ></ElButton>
      <ElButton
        type="success"
        icon="el-icon-caret-bottom"
        circle
        @click="sort('INCOME')"
      ></ElButton>
      <ElButton
        type="success"
        icon="el-icon-caret-top"
        circle
        @click="sort('OUTCOME')"
      ></ElButton>
      <template v-if="!isEmpty">
        <div class="list-item" v-for="(item, prop) in list" :key="prop">
          <template v-if="item.type === sort_new">
            <span class="budget-comment">
              <i
                :class="[
                  item.type === 'OUTCOME' ? elIconCaretBottom : elIconCaretTop
                ]"
              ></i>
              {{ item.comment }}</span
            >
            <span class="budget-value">{{ item.value }}</span>
            <ElButton
              type="danger"
              size="mini"
              @click="deleteItem(item.id, item.comment, item.value)"
              >Delete</ElButton
            >
          </template>

          <template v-if="sort_new === true">
            <span class="budget-comment">
              <i
                :class="[
                  item.type === 'OUTCOME' ? elIconCaretBottom : elIconCaretTop
                ]"
              ></i>
              {{ item.comment }}</span
            >
            <span class="budget-value">{{ item.value }}</span>
            <ElButton
              type="danger"
              size="mini"
              @click="deleteItem(item.id, item.comment, item.value)"
              >Delete</ElButton
            >
          </template>
        </div>
        <modal-first
          ref="dialog"
          @confirm-answer="answerTrue"
          :message="message"
        ></modal-first>
      </template>
      <ElAlert v-else type="error" :title="emptyTitle"></ElAlert>
    </ElCard>
  </div>
</template>

<script>
import Dialog from "@/components/Dialog";
export default {
  name: "BudgetList",
  components: {
    "modal-first": Dialog
  },

  props: {
    list: {
      type: Object,
      default: () => ({})
    }
  },
  data: () => ({
    header: "Budget List ",
    header2: '<i class="el-icon-top"></i>',
    emptyTitle: "Empty",
    visible: false,
    message: {
      id: null,
      comment: "",
      value: ""
    },
    sort_new: true,
    elIconCaretBottom: "el-icon-caret-bottom",
    elIconCaretTop: "el-icon-caret-top"
  }),
  computed: {
    isEmpty() {
      return !Object.keys(this.list).length;
    }
  },
  methods: {
    deleteItem(id, comment, value) {
      this.openDialog();
      this.message.id = id;
      this.message.comment = comment;
      this.message.value = value;
    },

    sort(value) {
      return (this.sort_new = value);
    },

    answerTrue(value) {
      if (value === true) this.$emit("deleteItem", this.message.id);
    },
    openDialog() {
      if (this.$refs["dialog"]) this.$refs["dialog"].open();
    }
  }
};
</script>

<style scoped>
.budget-list-wrap {
  max-width: 500px;
  margin: 0 auto;
}
.list-item {
  display: flex;
  align-items: center;
  padding: 10px 15px;
}
.budget-value {
  font-weight: bold;
  margin-left: auto;
  margin-right: 20px;
}
</style>
