
<script setup>
import { defineProps, defineEmits } from 'vue';

defineProps({
  type: { required: true },
    label: { required: true },
    name: { required: true },
    modelValue: String,
    error: { required: true }
})
const emit = defineEmits(['validate', 'update:modelValue'])
</script>

<template>
  <div :class="[ 'form-group', !!error && 'has-error' ]">
    <label class="form-label" for="email">{{label}}</label>
    <input
      :id="name"
      :name="name"
      :type="type"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      class="form-input"
      @blur="$emit('validate')"
      @keypress="$emit('validate')"
    />
    <p class="form-input-hint" v-if="!!error">{{ error }}</p>
  </div>
</template>