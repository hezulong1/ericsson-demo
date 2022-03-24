<script lang="ts">
import type { PropType } from 'vue'
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    modelValue: String as PropType<string>,
    options: {
      type: Array as PropType<{ code: string; name: string }[]>,
      default: () => []
    }
  },
  emits: [
    'update:modelValue'
  ],
  methods: {
    handleChange($event: Event) {
      const value = $event ? ($event.target as any).value : ''
      this.$emit('update:modelValue', value)
    }
  }
})
</script>

<template>
  <select :value="modelValue" class="s-select" @change="handleChange">
    <option disabled value="">
      请选择
    </option>
    <option v-for="(d, i) in options" :key="`option-${ i }`" :value="d.code">
      {{ d.name }}
    </option>
  </select>
</template>

<style>
.s-select {
  min-width: 120px;
  border: 1px solid currentColor;
}

.s-select:focus {
  outline: 1px solid currentColor;
}
</style>
