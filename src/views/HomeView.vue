<script setup lang="ts">
import GridElement from '@/components/GridElement.vue'
import { onMounted, ref } from 'vue'

const dragItem = ref<any>()
interface IDragItem {
  id?: number
  img?: string
}
const items = ref<IDragItem[]>([
  { id: 0, img: '/src/components/icons/GreenIcon.svg' },
  { id: 1, img: '/src/components/icons/YellowIcon.svg' },
  { id: 2, img: '/src/components/icons/PurpleIcon.svg' }
])

</script>

<template>
  <main>
    <div class="main">
      <div class="container">
        <div class="content">
          <div class="left">
            <div class="left__wrapper">
              <div class="left__top">
                <img src="../assets/image.svg" alt="" />
              </div>
              <div class="left__content">
                <div class="left__line left__line_wide loading"></div>
                <div
                  class="left__line loading"
                  v-for="i in 5"
                  :key="i"
                  :style="{ width: `${Math.floor(Math.random() * 200)}px` }"
                ></div>
              </div>
            </div>
          </div>
          <div class="drop-zone">
            <grid-element
              v-for="(item, index) in items"
              :key="item.id"
              :img="item.img"
              @dragstart="handleDragStart(index)"
              @dragend="handleDragEnd"
              @dragover.prevent
              @drop="handleDrop(index)"
            >
            </grid-element>
            <!-- <div
              class="drop-el"
              v-for="(item, index) in items"
              :key="item.id"
              :draggable="Object.keys(item).length === 0 ? false : true"
              @dragstart="handleDragStart(index)"
              @dragend="handleDragEnd"
              @dragover.prevent
              @drop="handleDrop(index)"
            >
              {{ item.title }}
            </div> -->
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped lang="scss">
@import '../assets/main.scss';
#app {
  color: black;
  margin-top: 60px;
}
.left {
  background-color: $color-background-items;
  border: 1px solid $color-border;
  border-radius: 12px;
  padding: 24px;
  &__wrapper {
  }

  &__top {
    margin: 0 0 24px;
  }

  &__content {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 24px;
  }

  &__line {
    width: 155px;
    height: 10px;
    border-radius: 10px;
    &_wide {
      width: 190px;
      height: 26px;
    }
  }
}
.loading {
}

.drop-zone {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: repeat(5, 1fr);
  border: 1px solid #4d4d4d;
  border-radius: 12px;
  overflow: hidden;
  background-color: $color-background-items;
  min-height: 100px;
}

.drop-el {
  background-color: #eee;
  border: 1px solid gray;
}

.content {
  margin-top: 100px;
  display: grid;
  grid-template-columns: 0.5fr 1fr;
  gap: 40px;
}
</style>
