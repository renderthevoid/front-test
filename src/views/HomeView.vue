<script setup lang="ts">
import AppButton from '@/components/AppButton.vue'
import AppInput from '@/components/AppInput.vue'
import AsideBar from '@/components/AsideBar.vue'
import GridElement from '@/components/GridElement.vue'
import { onMounted, ref } from 'vue'

const dragItem = ref<any>()
const modalContent = ref<IDragItem>({})
const isModalClose = ref<boolean>(false)
const isCountModal = ref<boolean>(false)

const counterValue = ref<string | number>('')
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
const closeModal = () => {
  isModalClose.value = true
  isCountModal.value = false
}
const elementModal = (item: IDragItem) => {
  isCountModal.value = false
  modalContent.value = item
  isModalClose.value = false
}
const setCounterValue = (item: IDragItem) => {
  items.value = items.value.map((el) => {
    if (el.id === item.id) {
      return { ...el, counter: Number(counterValue.value) }
    }
    return el
  })
}
const counterModal = (item: IDragItem) => {
  counterValue.value = ''
  isCountModal.value = true
  modalContent.value = item
  isModalClose.value = false
}
const deleteElement = (item: IDragItem) => {
  items.value = items.value.map((el) => {
    if (el.id === item.id) {
      return {}
    }
    return el
  })
  isModalClose.value = true
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
              <div class="left__content loading">
                <div class="left__line left__line_wide loading"></div>
                <div
                  class="left__line"
                  v-for="i in 5"
                  :key="i"
                  :style="{ width: `${Math.floor(Math.random() * 200)}px` }"
                ></div>
              </div>
            </div>
          </div>
          <div class="drop-zone">
            <template v-if="Object.keys(modalContent).length && !isModalClose">
              <aside-bar :img-src="modalContent?.img" @close-modal="closeModal">
                <template v-if="isCountModal">
                  <div class="actions">
                    <div class="actions__input">
                      <app-input
                        placeholder="Введите количество"
                        v-model="counterValue"
                      ></app-input>
                    </div>
                    <div class="actions__btns">
                      <app-button mode="primary" @click="counterValue = ''">Отмена</app-button>
                      <app-button mode="negative" @click.prevent="setCounterValue(modalContent)"
                        >Подтвердить</app-button
                      >
                    </div>
                  </div>
                </template>
                <template v-else>
                  <app-button mode="negative" @click="deleteElement(modalContent)"
                    >Удалить предмет</app-button
                  >
                </template>
              </aside-bar>
            </template>
            <div
              class="draggable-element"
              v-for="(item, index) in items"
              :key="item.id"
              :draggable="Object.keys(item).length === 0 ? false : true"
              @dragstart="handleDragStart(index)"
              @dragend="handleDragEnd"
              @dragover.prevent
              @drop="handleDrop(index)"
            >
              <grid-element
                :img="item.img"
                :counter="item.counter"
                @click="elementModal(item)"
                @counter-click="counterModal(item)"
              >
              </grid-element>
            </div>
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
    border-radius: 4px;
    animation-delay: .06s;
    &_wide {
      width: 190px;
      height: 26px;
    }
  }
}
@keyframes loading {
  to {
    background-position-x: -20%;
  }
}

.loading {
  background-color: white;
  background: linear-gradient(
      100deg,
      rgba(255, 255, 255, 0) 40%,
      rgba(255, 255, 255, 0.5) 50%,
      rgba(255, 255, 255, 0) 60%
    )
    var(--loading-grey);
  background-size: 200% 100%;
  background-position-x: 180%;
  animation: 1s loading ease-in-out infinite;
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

.actions {
  position: relative;
  bottom: 60px;
  background-color: $color-background-items;
  padding: 20px 0;
  border-top: 1px solid $color-border;
  &__input {
    margin: 0 0 20px;
  }
  &__btns {
    display: flex;
    gap: 10px;
  }
}
</style>
