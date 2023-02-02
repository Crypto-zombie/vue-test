<script setup>
import { object, string } from "yup";
import { ref } from "vue";
import FormInput from "./components/FormInput.vue";
const loginFormSchema = object().shape({
  email: string().email().required(),
  password: string().required(),
});
const email = ref('')
const password = ref('')
const e_email = ref('')
const e_pass = ref('')

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
        v-model="email"
        type="email"
        @validate="validate('email')"
        name="email"
        :error="e_email"
      />
      <FormInput
        label="Password"
        v-model="password"
        type="password"
        @validate="validate('password')"
        name="password"
        :error="e_pass"
      />
      <button class="btn btn-primary btn-block">Login</button>
    </form>
  </div>
</template>
