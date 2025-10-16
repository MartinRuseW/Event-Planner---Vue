<template>
  <div class="d-flex flex-column">
    <label :for="fieldId" class="form-label">{{ label }}<span v-if="isRequired" class="text-danger">*</span></label>
    <div class="input-error-container">
      <Field v-bind="fieldAttrs" :name="name" :value="modelValue" @input="updateFieldValue" class="form-control"
        autocomplete="on" />
      <ErrorMessage :name="name" class="text-danger position-absolute align-self-end error-message" />
    </div>
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
  name: { type: String, default: '' },
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

.form-control {
  margin-bottom: 0.25rem;
}

.input-error-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  position: relative;

  .form-control {
    width: 100%;
  }

  .error-message {
    margin-top: 2.5rem;
    align-self: flex-end;
    place-self: center end;
  }
}
</style>
