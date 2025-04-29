<template>
  <div class="grid grid-cols-2 py-4 border-b border-gray-200 dark:border-gray-800 text-gray-900 dark:text-gray-100">
    <div class="flex items-center justify-between">
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
          <UButton color="neutral" variant="ghost" trailing-icon="i-heroicons-ellipsis-horizontal" />
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
  };
}>();

const isIncome = computed(() => props.transaction.type === 'Income')

const icon = computed(
  () => isIncome.value ? 'i-heroicons-arrow-up-right' : 'i-heroicons-arrow-down-left'
)

const iconColor = computed(
  () => isIncome.value ? 'text-green-600' : 'text-red-600'
)

const { currency } = useCurrency(props.transaction.amount)


const items = ref<DropdownMenuItem[][]>([
  [
    {
      label: 'Profile',
      icon: 'i-lucide-user',
      onSelect() {
        console.log('Invite by email clicked')
      }
    },
    {
      label: 'Settings',
      icon: 'i-lucide-cog'
    }
  ],
  [
    {
      label: 'Logout',
      icon: 'i-lucide-log-out'
    }
  ]
])
</script>