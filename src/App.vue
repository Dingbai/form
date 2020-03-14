<template>
  <s-form :models="form" ref="form" class="form">
    <s-form-item label="姓名" prop="name" :rule="{ required: true }">
      <s-input v-model="form.name" placeholder="请输入姓名"></s-input>
    </s-form-item>
    <s-form-item label="年龄" prop="age" :rule="{ required: true }">
      <s-input v-model="form.age" placeholder="请输入年龄"></s-input>
    </s-form-item>
    <s-form-item label="邮箱" prop="email" :rule="{ type: 'email' }">
      <s-input v-model="form.gender" placeholder="请输入email"></s-input>
    </s-form-item>
    <div class="submit-button">
      <button @click="submit">submit</button>
      <button @click="reset">reset</button>
    </div>
  </s-form>
</template>

<script>
import sForm from "@/components/form/form.vue";
import sFormItem from "@/components/form/form-item.vue";
import sInput from "@/components/input/input.vue";

export default {
  name: "Home",
  components: {
    sForm,
    sFormItem,
    sInput
  },
  data() {
    return {
      form: {
        name: "",
        email: "",
        age: ""
      }
    };
  },
  methods: {
    reset() {
      this.$refs.form.resetField();
    },
    submit() {
      this.$refs.form.validateField(valid => {
        if (valid) {
          window.alert("提交成功");
        } else {
          window.alert("表单校验失败");
        }
      });
    }
  }
};
</script>
<style lang="less" scoped>
.form {
  padding: 20px;
  width: 360px;
  border: 1px solid #ddd;
  margin: 100px auto;
}
.submit-button {
  width: 100%;
  text-align: right;
}
button {
  margin-left: 20px;
  text-align: right;
  display: inline-block;
  margin-bottom: 0;
  font-weight: 400;
  text-align: center;
  vertical-align: middle;
  -ms-touch-action: manipulation;
  touch-action: manipulation;
  cursor: pointer;
  background-image: none;
  border: 1px solid transparent;
  white-space: nowrap;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  height: 32px;
  padding: 0 15px;
  font-size: 14px;
  border-radius: 4px;
  transition: color 0.2s linear, background-color 0.2s linear,
    border 0.2s linear, box-shadow 0.2s linear;
  color: #515a6e;
  background-color: #fff;
  border-color: #dcdee2;
}
</style>
