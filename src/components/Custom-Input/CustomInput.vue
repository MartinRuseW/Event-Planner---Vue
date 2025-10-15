<template>
  <div class="mb-4">
    <label :for="fieldId" class="form-label"
      >{{ label }}<span v-if="isRequired" class="text-danger">*</span></label
    >
    <Field
      v-bind="fieldAttrs"
      :name="name"
      :value="modelValue"
      @input="updateFieldValue"
      class="form-control"
      autocomplete="on"
    />
    <ErrorMessage :name="name" class="text-danger" />
  </div>
</template>

<script setup>
import { Field, ErrorMessage } from 'vee-validate';

const props = defineProps({
  fieldAttrs: {
    type: Object,
    default: {}
  },
  label: String,
  type: String, 
  fieldId: String,
  placeholderValue: String,
  isRequired: Boolean,
  modelValue: [String, Number, Date],
  name: {type: String, default: ''},
});

const emit = defineEmits();

const updateFieldValue = (event) => {
  emit('update:modelValue', event.target.value);
};
</script>

<style lang="scss" scoped>
label {
  color: $classic-cream;
}
</style>
