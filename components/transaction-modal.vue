<template>
  <div>
    <UModal v-model:open="isOpen">
      <template #header>
        <p>oi</p>
      </template>
      <template #content>
        <div class="flex flex-col gap-4 p-4">
          <UForm :state="state" :schema="schema" ref="form" @submit.prevent="save">
            <UFormField :required="true" label="Transaction Type" name="type" class="mb-4">
              <USelect placeholder="Select the Transaction Type" :items="types" v-model="state.type" />
            </UFormField>
            <UFormField label="Amount" :required="true" name="amount" class="mb-4">
              <UInput type="number" placeholder="Enter amount" v-model.number="state.amount" />
            </UFormField>
            <UFormField label="Transaction date" :required="true" name="created_at" class="mb-4">
              <UInput type="date" icon="i-heroicons-calendars-days-20-solid" v-model="state.created_at" />
            </UFormField>
            <UFormField label="Description" hint="Optional" name="description" class="mb-4">
              <UInput placeholder="Description" v-model="state.description" />
            </UFormField>
            <UFormField :required="true" label="Category" name="category" class="mb-4" v-if="state.type === 'Expense'">
              <USelect placeholder="Category" :items="categories" v-model="state.category" />
            </UFormField>
            <UButton :loading="isLoading" type="submit" class="cursor-pointer" color="info" variant="solid"
              label="Save" />
          </UForm>
        </div>
      </template>
    </UModal>
  </div>
</template>


<script lang="ts" setup>
import { categories } from '~/constants';
import { types } from '~/constants';
import { z } from 'zod'

const props = defineProps({
  modelValue: {
    type: Boolean,
    required: true
  }
});

const defaultSchema = z.object({
  created_at: z.string(),
  description: z.string().optional(),
  amount: z.number().positive(),
})

const incomeSchema = z.object({
  type: z.literal('Income')
})

const expenseSchema = z.object({
  type: z.literal('Expense'),
  category: z.enum(categories as [string, ...string[]])
})

const investimentSchema = z.object({
  type: z.literal('Investiment'),
})

const savingSchema = z.object({
  type: z.literal('Saving'),
})

const schema = z.intersection(
  z.discriminatedUnion('type', [incomeSchema, expenseSchema, investimentSchema, savingSchema]),
  defaultSchema
)

const form = ref()
const isLoading = ref(false)
const supabase = useSupabaseClient()
const toast = useToast()
const save = async () => {
  if (form.value.errors.length) return

  isLoading.value = true
  try {
    const { error } = await supabase.from('transactions')
      .upsert({ ...state.value })
    if (!error) {
      toast.add({
        'title': 'Transaction saved',
        'icon': 'i-heroicons-check-circle'
      })
      isOpen.value = false
      emit('update:modelValue', false)
      return
    }

    throw error

  } catch (e) {
    toast.add({
      title: 'Transaction not saved',
      description: e instanceof Error ? e.message : String(e),
      icon: 'i-heroicons-exclamation-circle',
      color: 'secondary'
    })
  } finally {
    isLoading.value = false
  }
}

const emit = defineEmits<{
  (e: 'update:modelValue', value: boolean): void
}>()

type Transaction = {
  type?: 'Income' | 'Expense' | 'Investiment' | 'Saving';
  amount: number;
  created_at?: string;
  description?: string;
  category?: string;
}

const initialState: Transaction = {
  type: undefined,
  amount: 0,
  created_at: undefined,
  description: undefined,
  category: undefined
}

const state = ref<Transaction>({ ...initialState })

const resetForm = () => {
  Object.assign(state.value, initialState)
  form.value.clear()
}

const isOpen = computed({
  get: () => props.modelValue,
  set: (value) => {
    if (!value) resetForm()
    emit('update:modelValue', value)
  }
})
</script>

<style></style>