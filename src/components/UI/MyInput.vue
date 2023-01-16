<template>
  <input
    v-focus
    type="text"
    class="add-name-folder"
    :value="modelValue"
    @input="updateInput"
  />
</template>

<script>
export default {
  props: {
    modelValue: [String, Number],
  },
  methods: {
    updateInput(e) {
      this.$emit("update:modelValue", e.target.value);
    },
  },
  directives: {
    focus: {
      // определение директивы
      mounted(el) {
        el.focus();
      },
    },
  },
};
</script>

<style scoped lang="scss">
input {
  width: 100%;
  background: #ffffff;
  border: 1px solid #efefef;
  border-radius: 4px;
  padding: 7px 83px 7px 11px;
  font-family: "Lato";
  font-style: normal;
  font-weight: 600;
  font-size: 14px;
  letter-spacing: 0.15px;
  color: #c7c7c7;
}
</style>
