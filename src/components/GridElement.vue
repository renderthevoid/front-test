<template>
  <div class="element">
    <div class="element__wrapper">
      <div class="element__image">
        <img :src="props.img" alt="">
      </div>
      <div class="element__counter">
        <div class="element__counter-item">4</div>
      </div>
      <slot></slot>
    </div>
  </div>
</template>

<script setup lang="ts">
interface IGridElementProps {
  id?: number
  title?: string
  img?: string
}
const props = defineProps<IGridElementProps>()
</script>

<style scoped lang="scss">
@import '../assets/main.scss';
.element {
  position: relative;
  background-color: $color-background-items;
  border: 1px solid $color-border;

  -webkit-user-select: none; /* Chrome/Safari */
  -moz-user-select: none; /* Firefox */
  -ms-user-select: none; /* IE/Edge */
  user-select: none; /* Стандартный синтаксис */
  &__wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    padding: 24px;
  }

  &__image {
  }

  &__counter {
    position: absolute;
    bottom: 0;
    right: 0;
    padding: 4px 8px;
    border: 1px solid $color-border;
    border-radius: 6px 0 0 0;
  }

  &__counter-item {
    font-weight: 500;
    font-size: 10px;
    text-align: center;
    color: #fff;
    opacity: 0.4;
  }
}
</style>
