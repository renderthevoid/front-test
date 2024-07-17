<template>
  <aside class="modal">
    <div class="container">
      <div class="modal-right">
        <div class="modal-right__wrapper">
          <div class="modal-right__top">
            <div class="modal-right__top-wrap">
              <button type="button" class="modal-right__close" @click="closeModal">
                <span>
                  <svg
                    width="12"
                    height="12"
                    viewBox="0 0 12 12"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <path
                      d="M12 1.05L10.95 0L6 4.95L1.05 0L0 1.05L4.95 6L0 10.95L1.05 12L6 7.05L10.95 12L12 10.95L7.05 6L12 1.05Z"
                      fill="white"
                    />
                  </svg>
                </span>
              </button>
            </div>
          </div>
          <div class="modal-right__content">
            <div class="modal-right__image">
              <img :src="imgSrc" alt="" />
            </div>
            <div class="modal-right__lines">
              <div
                class="left__line loading"
                v-for="i in 5"
                :key="i"
                :style="{ width: `${Math.floor(Math.random() * 200)}px` }"
              ></div>
            </div>
          </div>
          <div class="modal-right__bottom">
            <slot></slot>
          </div>
        </div>
      </div>
    </div>
  </aside>
</template>

<script setup lang="ts">
interface IAsideBarProps {
  imgSrc?: string
}
const emit = defineEmits(['closeModal'])
const props = defineProps<IAsideBarProps>()

const closeModal = () => {
  emit('closeModal')
}
</script>

<style scoped lang="scss">
@import '../assets/main.scss';
aside {
  position: absolute;
  height: 100%;
  width: 100%;
}
.container {
  position: relative;
  height: 100%;
}
.modal-right {
  padding: 24px;
  position: absolute;
  height: fit-content;
  right: 0;
  width: 20vw;
  background-color: $color-background-items;
  border: 1px solid $color-border;
  border-radius: 12px;
  padding: 24px;
  z-index: 10;
  &__top {
    margin: 0 0 24px;
  }
  &__top-wrap {
    display: flex;
    justify-content: flex-end;
    width: 100%;
  }
  &__lines {
    padding: 16px 0;
  }
  &__close {
    cursor: pointer;
  }
  &__image {
    padding: 0 0 30px;
    border-bottom: 1px solid $color-border;
    img {
      width: 130px;
      height: 130px;
    }
  }

  &__lines {
    display: flex;
    flex-direction: column;
    gap: 24px;
    align-items: center;
  }
}
</style>
