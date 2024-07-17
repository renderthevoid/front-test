<template>
  <button type="button" class="button" :class="mode ? `button_${mode}` : ''">
    <span class="button__content">
      <slot></slot>
    </span>
  </button>
</template>

<script setup lang="ts">
interface IAppButtonProps {
  mode?: string
}

const props = defineProps<IAppButtonProps>()
</script>

<style scoped lang="scss">
.button {
  padding: 11px;
  background: #f88;
  border-radius: 8px;
  width: 100%;
  box-shadow: -1px 1px 100px 0px #fa7272;;
  cursor: pointer;
  &_primary {
    background: #fff;
    .button__content {
      color: #000;
    }
  }
  &_negative {
    background: #f88;
    .button__content {
      color: #fff;
    }
  }
  &__content {
    font-weight: 400;
    font-size: 14px;
    text-align: center;
  }
}
</style>
