<script setup>
import { object, string } from "yup";
import { reactive } from "vue";
import FormInput from "./components/FormInput.vue";
const loginFormSchema = object().shape({
  email: string().email().required('Email cannot be empty'),
  password: string()
    .min(8,'Password should be less than 8 characters')
    .required('Password cannot be empty'),
});
const info = reactive({
  email: '',
  password: '',
})
const errors = reactive({
  email: '',
  password: '',
})

const loginUser = () => {
  loginFormSchema
    .validate(info, { abortEarly: false })
    .then()
    .catch((err) => {
      err.inner.forEach((error) => {
        errors = { ...errors, [error.path]: error.message };
      });
    });
};
const validate = (field) => {
  loginFormSchema
    .validateAt(field, info)
    .then(() => {
      errors[field] = "";
    })
    .catch((err) => {
      errors[err.path] = err.message;
    });
};
</script>

<template>
  <div id="app">
    <form class="login-form" @submit.prevent="loginUser">
      <h2>Login</h2>
      <FormInput
        label="Email"
        v-model="info.email"
        type="email"
        @validate="validate('email')"
        name="email"
        :error="errors.email"
      />
      <FormInput
        label="Password"
        v-model="info.password"
        type="password"
        @validate="validate('password')"
        name="password"
        :error="errors.password"
      />
      <button class="btn btn-primary btn-block">Login</button>
    </form>
  </div>
</template>
