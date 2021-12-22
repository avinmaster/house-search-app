<template>
  <div class="group">
    <label for="input">{{ label }}</label>
    <input
      id="input"
      type="number"
      @input="$emit('input', $event.target.value)"
      min="0"
    />
  </div>
</template>

<script>
export default {
  name: "InputComponent",
  props: {
    label: {
      type: String,
    },
  },
};
</script>
