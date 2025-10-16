<template>
  <div class="container mt-4">
    <div class="row justify-content-center">
      <div class="col-md-6">
        <div class="card custom-card">
          <div class="card-header custom-card-header">
            <h3>Register</h3>
          </div>
          <div class="card-body">
            <div v-if="registerError" class="alert alert-danger">
              {{ registerError }}
            </div>
            <Form @submit="onSubmit" :validationSchema="validation">
              <div class="mb-4">
                <CustomInput :fieldAttrs="{
                  type: 'text',
                  id: 'username',
                  placeholder: 'Enter username...',
                }" label="Username" name="username" is-required v-model="formData.username" />
              </div>
              <div class="mb-4">
                <CustomInput :fieldAttrs="{
                  type: 'text',
                  id: 'email',
                  placeholder: 'Enter email...',
                }" label="Email" name="email" is-required v-model="formData.email" />
              </div>
              <div class="mb-4">
                <Dropdown @selectZone="handleTimeZone" id="location" :name="'location'" :label="'Location'"></Dropdown>
              </div>
              <div class="mb-4">
                <CustomInput :fieldAttrs="{
                  type: 'password',
                  id: 'password',
                  placeholder: 'Enter password...',
                }" label="Password" name="password" is-required v-model="formData.password" />
              </div>
              <div class="mb-4">
                <CustomInput :fieldAttrs="{
                  type: 'password',
                  id: 'repeatPassword',
                  placeholder: 'Repeat the password...',
                }" label="Repeat password" name="repeatPassword" is-required v-model="formData.repeatPassword" />
              </div>
              <div class="text-center">
                <button type="submit" class="btn custom-btn" btn-style="default-button-small">
                  Register
                </button>
              </div>
            </Form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue';
import { useRouter } from 'vue-router';
import { Form } from 'vee-validate';
import * as yup from 'yup';

import CustomInput from '@/components/Custom-Input/CustomInput.vue';
import Dropdown from '@/components/Dropdown/Dropdown.vue';
import userServices from '@/services/users/userServices.js';
import showNotification from '@/utils/notifications/showNotification.js';
import { minPasswordLength, minUsernameLength } from '@/utils/constants.js';

const router = useRouter();
const registerError = ref('');

const formData = reactive({
  username: '',
  email: '',
  password: '',
  repeatPassword: '',
});

const location = ref('');

const validation = yup.object({
  username: yup
    .string()
    .required('Username is required!')
    .min(
      minUsernameLength,
      `Username must be at least ${minUsernameLength} characters!`
    ),
  email: yup
    .string()
    .required('Email is required!')
    .email('Invalid email format!'),
  password: yup
    .string()
    .required('Password is required!')
    .min(
      minPasswordLength,
      `Password must be at least ${minPasswordLength} characters!`
    ),
  repeatPassword: yup
    .string()
    .required('Repeat password is required!')
    .oneOf([yup.ref('password')], 'Passwords must match!'),
});

const handleTimeZone = (selectedZone) => {
  location.value = selectedZone;
};

const onSubmit = async () => {
  try {
    await userServices.signUp(
      {
        email: formData.email,
        username: formData.username,
        location: location.value,
      },
      formData.password
    );
    router.push({ name: 'events' });
  } catch (error) {
    registerError.value = error.message || 'Registration failed!';
    showNotification(registerError.value, 5000);
  }
};
</script>

<style scoped lang="scss">
.custom-card {
  background-color: $lighter-gray;

  .custom-card-header {
    background-color: $dark-gray;
    color: $classic-cream;
  }

  .custom-btn {
    background-color: $elegant-gold;
    border: none;
  }
}
</style>
