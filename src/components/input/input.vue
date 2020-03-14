<template>
  <input
    type="text"
    class="s-input"
    :value="newValue"
    :placeholder="placeholder"
    @input="inputHandler"
    @blur="blurHandle"
  />
</template>

<script>
export default {
  name: "s-input",
  props: {
    value: { type: String },
    placeholder: { type: String, default: "" }
  },
  data() {
    return {
      newValue: this.value
    };
  },
  watch: {
    value(val) {
      this.newValue = val;
    }
  },
  methods: {
    inputHandler(event) {
      this.newValue = event.target.value;

      this.$emit("input", this.newValue);

      this.$parent.$emit("change-handle");
    },
    blurHandle() {
      this.$parent.$emit("blur-handle");
    }
  }
};
</script>

<style lang="less" scoped>
.s-input {
  width: 300px;
  height: 32px;
  line-height: 1.5;
  padding: 4px 7px;
  font-size: 14px;
  box-sizing: border-box;
}
</style>
