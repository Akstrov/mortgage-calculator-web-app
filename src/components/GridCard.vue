<script setup>
import Slider from 'primevue/slider'
</script>

<template>
  <div class="col-4 p-5">
    <div class="surface-card shadow-2 p-3 border-round">
      <div class="flex justify-content-between mb-3">
        <div>
          <div v-if="type == 'price'" class="text-900 font-medium text-xl">
            <slot></slot> ${{ getValue }}
          </div>
          <div v-else-if="type == 'time'" class="text-900 font-medium text-xl">
            <slot></slot>{{ getValue }} years
          </div>
          <div v-else-if="type == 'percentage'" class="text-900 font-medium text-xl">
            <slot></slot>{{ getValue }}%
          </div>
          <div v-else-if="type == 'result'" class="text-900 font-medium text-xl">
            <slot></slot>
          </div>
        </div>
      </div>
      <Slider
        v-if="type == 'percentage' || type == 'price' || type == 'time'"
        v-model.number="value"
        class="w-auto"
        @change="$emit('updateValue', value)"
      />
      <p v-else>${{ getLoan }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      value: 0
    }
  },
  props: {
    type: {
      type: String
    },
    loan: {
      type: Number
    }
  },
  computed: {
    getValue: {
      get() {
        if (this.type == 'price') {
          return new Intl.NumberFormat('en-US').format(this.value * 10000)
        } else if (this.type == 'percentage') {
          return this.value
        } else if (this.type == 'time') {
          return Math.floor(this.value / 4)
        } else {
          return 0
        }
      }
    },
    getLoan: {
      get() {
        return new Intl.NumberFormat('en-US').format(this.loan)
      }
    }
  }
}
</script>
