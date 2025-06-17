<template>
  <div class="grid grid-cols-3 py-4 border-b border-gray-200 dark:border-gray-800 text-gray-900 dark:text-gray-100">
    <div class="flex items-center justify-between space-x-4 col-span-2">
      <div class="flex items-center space-x-1">
        <UIcon :name="icon" :class="[iconColor]" />
        <div>{{ transaction.description }}</div>
      </div>

      <div>
        <UBadge color="neutral" v-if="transaction.category">{{ transaction.category }}</UBadge>
      </div>
    </div>

    <div class="flex items-center justify-end space-x-2">
      <div>{{ currency }}</div>
      <div>
        <UDropdownMenu :items="items">
          <UButton color="neutral" variant="ghost" trailing-icon="i-heroicons-ellipsis-horizontal"
            :loading="isLoading" />
        </UDropdownMenu>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import type { DropdownMenuItem } from '@nuxt/ui';

const props = defineProps<{
  transaction: {
    description: string;
    category?: string;
    type: string;
    amount: number;
    id: number;
  };
}>();

const emit = defineEmits(['deleted'])

const isIncome = computed(() => props.transaction.type === 'Income')

const icon = computed(
  () => isIncome.value ? 'i-heroicons-arrow-up-right' : 'i-heroicons-arrow-down-left'
)

const iconColor = computed(
  () => isIncome.value ? 'text-green-600' : 'text-red-600'
)

const { currency } = useCurrency(props.transaction.amount)

const isLoading = ref(false)
const toast = useToast()
const supabase = useSupabaseClient()

const deleteTransaction = async () => {
  isLoading.value = true

  try {
    await supabase.from('transactions')
      .delete()
      .eq('id', props.transaction.id)
    toast.add({
      title: 'Transaction deleted',
      icon: 'i-lucide-circle',
      color: 'primary'
    })
    emit('deleted', props.transaction.id)
  } catch (error) {
    toast.add({
      title: 'Transaction deleted',
      icon: 'i-lucide-circle',
      color: 'error'
    })

  } finally {
    isLoading.value = false
  }
}


const items = ref<DropdownMenuItem[][]>([
  [
    {
      label: 'Edit',
      icon: 'i-lucide-edit',

    },
    {
      label: 'Delete',
      icon: 'i-lucide-delete',
      onSelect() {
        deleteTransaction()
      }
    }
  ]
])
</script>