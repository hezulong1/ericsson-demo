<script setup lang="ts">
import { computed, ref } from 'vue'
import provinces from '~/data/City.json'

interface IBase {
  code: string
  name: string
  province: string
}

interface IArea extends IBase {
  city: string
  area: string
}

interface ICity extends IBase {
  city: string
  children: Array<IArea>
}

interface IProvince extends IBase {
  children: Array<ICity>
}

const selectedProvince = ref('')
const selectedCity = ref('')
const selectedArea = ref('')

const cities = computed(() => {
  const targetProvince = (provinces as IProvince[]).find(d => d.code === selectedProvince.value)
  return targetProvince && Array.isArray(targetProvince.children) ? targetProvince.children : []
})

const areas = computed(() => {
  const targetCity = (cities.value).find(d => d.code === selectedCity.value)
  return targetCity && Array.isArray(targetCity.children) ? targetCity.children : []
})

// function findTarget<T>(code: string, source: Array<T>) {

// }
</script>

<template>
  <div>
    <select v-model="selectedProvince">
      <option v-for="d in provinces" :key="d.code" :value="d.code">
        {{ d.name }}
      </option>
    </select>

    <select v-model="selectedCity">
      <option v-for="d in cities" :key="d.code" :value="d.code">
        {{ d.name }}
      </option>
    </select>

    <select v-model="selectedArea">
      <option v-for="d in areas" :key="d.code" :value="d.code">
        {{ d.name }}
      </option>
    </select>
  </div>
</template>

<style scoped>
div {
  display: flex;
  column-gap: 10px;
}
</style>
