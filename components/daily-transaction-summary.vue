<template>
  <div class="grid grid-cols-2 py-4 border-b border-gray-200 dark:border-gray-800 text-gray-500 dark:text-gray-400 font-bold">
    <div class="flex items-center justify-between">
      {{ date }}
    </div>

    <div class="flex items-center justify-end mr-10 space-x-2">
      {{currency}}
    </div>
  </div>
</template>
<script setup lang="ts">


interface Transaction {
  type: 'Income' | 'Expense';
  amount: number
}

const props = defineProps<{
  date: String,
  transactions: Transaction[];
}>()

const sum = computed(() => {
  let total = 0;
  for (const transaction of props.transactions) {
    if (transaction.type === 'Income') {
      total += transaction.amount;
    } else {
      total -= transaction.amount;
    }
  }
  return total;
})

const {currency} = useCurrency(sum)
  
</script>