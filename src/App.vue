<template>
  <div id="app">
    <Form @submitForm="submitForm" />
    <TotalBalance :total="totalBalance" :colorbalance="colorBalance" />
    <BudgetList :list="list" @deleteItem="oneDeleteItem" />
    <Dialog />
  </div>
</template>

<script>
import BudgetList from "@/components/BudgetList";
import TotalBalance from "@/components/TotalBalance";
import Form from "@/components/Form";
import Dialog from "@/components/Dialog";

export default {
  name: "App",
  components: {
    BudgetList,
    TotalBalance,
    Form,
    Dialog
  },
  data: () => ({
    list: {
      1: {
        type: "INCOME",
        value: 120,
        comment: "Some comment",
        id: 1
      },
      2: {
        type: "OUTCOME",
        value: -50,
        comment: "Some outcome comment",
        id: 2
      }
    }
  }),

  computed: {
    totalBalance() {
      return Object.values(this.list).reduce(
        (acc, item) => (acc += item.value),
        0
      );
    },

    colorBalance() {
      if (this.totalBalance > 0) {
        return "green";
      }
      if (this.totalBalance < 0) {
        return "red";
      }
      return "black";
    }
  },

  methods: {
    oneDeleteItem(id) {
      this.$delete(this.list, id);
    },

    submitForm(data) {
      const newObject = {
        ...data,
        id: String(Math.random())
      };

      if (newObject.type === "OUTCOME") {
        if (Math.sign(newObject.value) !== -1) {
          newObject.value = newObject.value * -1;
        }
      }

      this.$set(this.list, newObject.id, newObject);
    }
  }
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
