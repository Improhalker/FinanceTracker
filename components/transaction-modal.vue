<template>
  <div>
    <UModal v-model:open="isOpen">
      <template #header>
        <p>oi</p>
      </template>
      <template #content>
        <div class="flex flex-col gap-4 p-4">
          <UFormField :required="true" label="Transaction Type" name="type" class="mb-4">
            <USelect placeholder="Select the Transaction Type" :items="types"/>
          </UFormField>


          <UFormField label="Amount" :required="true" name="amount" class="mb-4">
            <UInput type="number" placeholder="Enter amount" />
          </UFormField>
          <UFormField label="Transaction date" :required="true" name="created_at" class="mb-4">
            <UInput type="date" icon="i-heroicons-calendars-days-20-solid" />
          </UFormField>
          <UFormField label="Description" hint="Optional" name="description" class="mb-4">
            <UInput placeholder="Description"/>
          </UFormField>
          <UFormField :required="true" label="Category" name="category" class="mb-4">
            <USelect placeholder="Category" :items="categories"/>
          </UFormField>

          <UButton type="submit" color="info" variant="solid" label="Save"/>
        </div>
      </template>
    </UModal>
  </div>
</template>


<script lang="ts" setup>
import { categories } from '~/constants';
import { types } from '~/constants';

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