<template>
  <input
    type="text"
    class="input"
    :value="props.modelValue"
    @input="updateInput"
    :placeholder="placeholder"
  />
</template>

<script setup lang="ts">
interface IAppInputProps {
  modelValue: string
  placeholder?: string
}
const props = defineProps<IAppInputProps>()
const emit = defineEmits(['update:modelValue'])
const updateInput = (event: Event) => {
  emit('update:modelValue', (event.target as HTMLInputElement).value)
}
</script>

<style scoped lang="scss">
@import "../assets/main.scss";
.input {
  border: 1px solid $color-border;
  border-radius: 4px;
  padding: 12px;
  font-weight: 500;
  font-size: 14px;
  color: #fff;
  width: 100%;
}
</style>
