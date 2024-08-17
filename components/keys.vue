<template>
<div class="grid grid-cols-3 gap-1 bg-green" style="width: fit-content">

  <button v-for="number in numbers" :key="number.id" 
          :value="number" 
          class="text-green  px-4 sm:px-8 py-2 sm:py-3 bg-sky-600 hover:bg-sky-900"
          @click="onNumClick">

          {{ number }}
  </button>
  <button @click="onDotClick" class="text-green  px-4 sm:px-8 py-2 sm:py-3 bg-sky-600 hover:bg-sky-900">
        .
  </button>

  <button @click="onClerClick" class="text-green  px-4 sm:px-8 py-2 sm:py-3 bg-sky-600 hover:bg-sky-900">
    C
  </button>
</div>
    
</template>

<script setup lang="ts">
const numbers = ref([1,2,3,4,5,6,7,8,9,0])

const emit = defineEmits<{
  update: [newValue: String],
  setdot: [newValue: String],
  clear: [newValue: String]
}>()

const props = defineProps({
  // value: { type: Number, required: true },
})

function onNumClick(event: any) {
  const value = event.target.value.toString()
  emit('update', value);
}

function onDotClick() {
  emit('setdot', '.');
}

function onClerClick() {
  emit('clear');
}

</script>

