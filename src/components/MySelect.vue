<template>
  <select :value="modelValue"
  @update="$emit('modelValue', event.target.value)"
  @change="changeOption">
    <option disabled value="">choose</option>
    <option v-for="option in options" :key="option.value" :value="option.value">{{
      option.name
    }}</option>
  </select>
</template>
<script>
export default {
  props: {
    modelValue: {
      type: String,
    },
    options: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    changeOption(event) {
      this.$emit('update:modelValue', event.target.value);
    },
  },
};
</script>
<style scoped></style>
