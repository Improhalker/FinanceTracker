<template>
  <div>
    <UModal v-model:open="isOpen">
      <template #header>
        Add Transaction
      </template>
      <template #content>
        Hello!
      </template>
    </UModal>
  </div>
</template>

<script lang="ts" setup>
const props = defineProps({
  modelValue: {
    type: Boolean,
    required: true
  }
});

const emit = defineEmits<{
  (e: 'update:modelValue', value: boolean): void
}>()

const isOpen = computed({
  get: () => props.modelValue,
  set: (value) => emit('update:modelValue', value)
})
</script>

<style></style>