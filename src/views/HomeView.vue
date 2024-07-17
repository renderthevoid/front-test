<script setup lang="ts">
import AsideBar from '@/components/AsideBar.vue'
import GridElement from '@/components/GridElement.vue'
import { onMounted, ref } from 'vue'

const dragItem = ref<any>()
const modalContent = ref<IDragItem>({})
const modalCloseState = ref<boolean>(false)
interface IDragItem {
  id?: number
  img?: string
  counter?: number
}
const items = ref<IDragItem[]>([
  { id: 0, img: '/src/components/icons/GreenIcon.svg', counter: 4 },
  { id: 1, img: '/src/components/icons/YellowIcon.svg', counter: 2 },
  { id: 2, img: '/src/components/icons/PurpleIcon.svg', counter: 5 }
])
function fillArrayWithEmptyObjects(array: IDragItem[], size: number) {
  // Проверяем, что размер массива меньше требуемого
  if (array.length < size) {
    const emptyObjectsCount = size - array.length
    const emptyObjects = Array(emptyObjectsCount).fill({})
    array.push(...emptyObjects)
  }
  return array
}

onMounted(() => {
  fillArrayWithEmptyObjects(items.value, 25)
})
const handleDragStart = (index: number) => {
  dragItem.value = index
}
const handleDragEnd = () => {
  dragItem.value = null
}
const handleDrop = (index: number) => {
  const droppedItem = items.value[dragItem.value]

  const temp = items.value[index]
  items.value[index] = droppedItem
  items.value[dragItem.value] = temp
}

const clickElement = (item: IDragItem) => {
  modalContent.value = item
  modalCloseState.value = false
}
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
            <template v-if="Object.keys(modalContent).length && !modalCloseState">
              <aside-bar :img-src="modalContent?.img" @close-modal="modalCloseState = true">
                <template v-if="modalContent?.counter">
                  <div class="modal-right__counter">{{ modalContent?.counter }}</div>
                </template>
              </aside-bar>
            </template>
            <div
              class="draggable-element"
              v-for="(item, index) in items"
              :key="item.id"
              @dragstart="handleDragStart(index)"
              @dragend="handleDragEnd"
              @dragover.prevent
              @drop="handleDrop(index)"
            >
              <grid-element :img="item.img" :counter="item.counter" @click="clickElement(item)">
              </grid-element>
            </div>
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
.main {
  position: relative;
}
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
  position: relative;
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
