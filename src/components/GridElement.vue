<template>
  <div class="element">
    <div class="element__wrapper">
      <div class="element__image">
        <img :src="props.img" alt="">
      </div>
      <div class="element__counter" v-if="typeof props.counter !== 'undefined'">
        <div class="element__counter-item">{{ props.counter }}</div>
      </div>
      <slot></slot>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface IGridElementProps {
  id?: number
  img?: string
  counter?: number | undefined
}
const props = defineProps<IGridElementProps>()

</script>

<style scoped lang="scss">
@import '../assets/main.scss';
.element {
  position: relative;
  height: 100%;
  background-color: $color-background-items;
  border: 1px solid $color-border;

  cursor: pointer;

  -webkit-user-select: none; 
  -moz-user-select: none;
  -ms-user-select: none; 
  user-select: none; 
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
