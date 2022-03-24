<script setup lang="ts">
import { computed, ref, watch } from 'vue'
import provinces from '~/data/City.json'

interface IBase {
  code: string
  name: string
  province: string
}

interface IArea extends IBase {
  city: string
  area: string
  children: Array<IBase>
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
const selectedTown = ref('')

const cities = computed(() => {
  const targetProvince = (provinces as IProvince[]).find(d => d.code === selectedProvince.value)
  return targetProvince && Array.isArray(targetProvince.children) ? targetProvince.children : []
})

const areas = computed(() => {
  const targetCity = (cities.value).find(d => d.code === selectedCity.value)
  return targetCity && Array.isArray(targetCity.children) ? targetCity.children : []
})

const towns = computed(() => {
  const targetArea = (areas.value).find(d => d.code === selectedArea.value)
  return targetArea && Array.isArray(targetArea.children) ? targetArea.children : []
})

watch(selectedProvince, (v) => {
  selectedCity.value = ''
})

watch(selectedCity, (v) => {
  selectedArea.value = ''
})

watch(selectedArea, (v) => {
  selectedTown.value = ''
})

// function findTarget<T>(code: string, source: Array<T>) {

// }
</script>

<template>
  <form>
    <fieldset>
      <legend>当前值：</legend>
      <p>
        省：{{ selectedProvince }}
      </p>
      <p>
        市: {{ selectedCity }}
      </p>
      <p>
        区: {{ selectedArea }}
      </p>
      <p>
        乡镇: {{ selectedTown }}
      </p>
    </fieldset>
    <select v-model="selectedProvince">
      <option disabled value="">
        Please select one
      </option>
      <option v-for="d in provinces" :key="d.code" :value="d.code">
        {{ d.name }}
      </option>
    </select>

    <select v-model="selectedCity">
      <option disabled value="">
        Please select one
      </option>
      <option v-for="d in cities" :key="d.code" :value="d.code">
        {{ d.name }}
      </option>
    </select>

    <select v-model="selectedArea">
      <option disabled value="">
        Please select one
      </option>
      <option v-for="d in areas" :key="d.code" :value="d.code">
        {{ d.name }}
      </option>
    </select>

    <select v-model="selectedTown">
      <option disabled value="">
        Please select one
      </option>
      <option v-for="d in towns" :key="d.code" :value="d.code">
        {{ d.name }}
      </option>
    </select>
  </form>
</template>

<style scoped>
div {
  display: flex;
  column-gap: 10px;
}
</style>
