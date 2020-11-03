<template>
  <div class="budget-list-wrap">
    <ElCard>
      <ElForm
        :model="formData"
        :rules="rules"
        ref="addItemForm"
        label-position="top"
      >
        <ElFormItem label="Type" prop="type">
          <ElSelect v-model="formData.type" placeholder="Choose type">
            <ElOption label="Income" value="INCOME"></ElOption>
            <ElOption label="Outcome" value="OUTCOME"></ElOption>
          </ElSelect>
        </ElFormItem>
        <ElFormItem label="Comments" prop="comment">
          <ElInput v-model="formData.comment" />
        </ElFormItem>
        <ElFormItem label="Value" prop="value">
          <ElInput v-model.number="formData.value" />
        </ElFormItem>
        <ElButton @click="onSubmit" type="primary">Submit</ElButton>
      </ElForm>
    </ElCard>
  </div>
</template>

<script>
export default {
  name: "Form",
  data() {
    const checkAge = (rule, value, callback) => {
      setTimeout(() => {
        if (value == 0) {
          callback(new Error("Value must be greater than 0"));
        } else {
          callback();
        }
      }, 500);
    };
    return {
      formData: {
        type: "INCOME",
        comment: "",
        value: 0
      },
      rules: {
        type: [
          { required: true, message: "Please select type", trigger: "change" }
        ],
        comment: [
          { required: true, message: "Please select number", trigger: "change" }
        ],
        value: [
          { validator: checkAge, trigger: "blur" },
          {
            type: "number",
            required: true,
            message: "Please input number more 0",
            trigger: "change"
          }
        ]
      }
    };
  },
  methods: {
    onSubmit() {
      this.$refs.addItemForm.validate(valid => {
        if (valid) {
          this.$emit("submitForm", { ...this.formData });
          this.$refs.addItemForm.resetFields();
        }
      });
    }
  }
};
</script>

<style scoped>
.budget-list-wrap {
  max-width: 500px;
  margin: 0 auto;
}
</style>
