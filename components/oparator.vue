<template>
 <div class="grid grid-cols-1 gap-1 bg-green " style="width: fit-content">
    <button v-for="oparator in oparators" :key="oparator.id" 
            :value="oparator" 
            class="text-green  px-4 sm:px-8 py-2 sm:py-3 bg-green-600 hover:bg-green-900"
            
            @click="handleButtonClick">
            {{ oparator }}
    </button>
 </div>
    
</template>

<script setup lang="ts">
const oparators = ref(['+', '-', '*', '/'])

const emit = defineEmits<{
  update: [newValue: string]
}>()

const props = defineProps({
  // value: { type: String, required: true },
})

const handleButtonClick = (event: any) => {
  emit('update', event.target.value);
};
</script>

