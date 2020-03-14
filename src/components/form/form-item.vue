<template>
  <div class="s-form-item">
    <span class="s-form-item_label" :class="requiredStyle">
      {{ label }}
    </span>
    <div class="s-form-item_control">
      <slot></slot>
      <p class="s-form-item_control_error">
        {{ validateMessage }}
      </p>
    </div>
  </div>
</template>
<script>
import schema from "async-validator";

export default {
  name: "s-form-item",
  props: {
    label: {
      type: String,
      default: ""
    },
    prop: {
      type: String,
      default: ""
    },
    rule: {
      type: Object
    }
  },
  inject: ["form"],
  data() {
    return {
      validateMessage: "",
      isRequired: false
    };
  },
  computed: {
    fieldValue() {
      return this.form.models[this.prop];
    },
    requiredStyle() {
      return this.isRequired ? "s-form-item_show" : "";
    }
  },
  mounted() {
    if (this.prop) {
      this.$parent.$emit("validate-item", this);
      // 保存初始值
      this.initialValue = this.fieldValue;
      // 处理rules
      this.setRules();
    }
  },
  methods: {
    setRules() {
      let rule = this.rule;
      if (rule.required) {
        this.isRequired = rule.required;
      }
      this.$on("blur-handle", () => {
        this.validate();
      });
      this.$on("change-handle", () => {
        this.validate();
      });
    },

    validate(callback = () => {}) {
      const validator = new schema({ [this.prop]: this.rule });
      const value = this.form.models[this.prop];
      const model = { [this.prop]: value };

      validator.validate(model, { firstFields: true }, errors => {
        this.validateMessage = errors ? errors[0].message : "";

        callback(this.validateMessage);
      });
    },
    reset() {
      this.validateMessage = "";

      this.form.models[this.prop] = this.initialValue;
    }
  }
};
</script>
<style scoped lang="less">
@import url("./form-item.less");
</style>
