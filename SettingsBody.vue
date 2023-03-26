<script setup lang="ts">
import {computed, onMounted, reactive, ref} from "vue"
import {drugStart, drugEnd, dragOver} from "@/features/dragAndDrop/drugEndDrop"
import {saveRefreshedLineToLS} from '../features/saveRefreshedLineToLS'
import SettingsDraggableCard from './SettingsDraggableCard.vue'

const weatherSets = reactive([
  {
    id: '773467',
    lon: '11',
    lat: '11'
  },
  {
    id: '86676',
    lon: '22',
    lat: '22'
  },
])
const emit = defineEmits(['reloadHimSelf'])

onMounted(() => {
  const tasksListElement: any = document.querySelector(`.weather-list`)
  
  tasksListElement.addEventListener(`dragstart`, (ev: any) => drugStart(ev))
  
  tasksListElement.addEventListener(`dragend`, (ev: any) => {
    let refreshedItemLine_id = drugEnd(ev, document, `.weather-list__item`)
    
    saveRefreshedLineToLS(refreshedItemLine_id, weatherSets.value)
    weatherStore.REFRESH_WEATHER_LINE(refreshedItemLine_id)
    
    emit('reloadHimSelf')
  })
  
  tasksListElement.addEventListener(`dragover`, (ev: any) => {
    dragOver(ev, tasksListElement, 'weather-list__item')
  })
})


</script>


<template>
  <div>
    <div class="weather-list">
      <SettingsDraggableCard v-for="(weatherSet, ind) of weatherSets"
                             :key="'sdc' + ind"
                             :weatherSet="weatherSet"
                             :id="weatherSet.id"
                             class="weather-list__item"
                             draggable="true"
      />
    </div>
    
  </div>
</template>


<style scoped lang="scss">
.weather-list {
}

.weather-list__item:not(:first-child) {
  margin-top: var(--vt-c-common-padding);
}

.weather-list:not(:first-child) {
  margin-top: 7px;
}

.selected {
  color: var(--vt-c-green);
  opacity: 0.6;
}

</style>