<template>
  <div class="s-form">
    <slot></slot>
  </div>
</template>
<script>
export default {
  name: "s-form",
  provide() {
    return {
      form: this
    };
  },
  props: {
    models: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      fields: []
    };
  },
  created() {
    // 保存 form-item 实例 为数组
    this.$on("validate-item", instance => {
      this.fields.push(instance);
    });
  },
  methods: {
    resetField() {
      this.fields.forEach(field => {
        field.reset();
      });
    },
    validateField(callback) {
      return new Promise(resolve => {
        let valid = true;
        let count = 0;

        this.fields.forEach(field => {
          field.validate(errors => {
            if (errors) {
              valid = false;
            }
            // 保证拿到所有验证结果
            if (++count === this.fields.length) {
              resolve(valid); // 支持promise 调用

              if (typeof callback === "function") {
                callback(valid);
              }
            }
          });
        });
      });
    }
  }
};
</script>
