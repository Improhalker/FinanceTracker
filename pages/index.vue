<template>
    <section class="flex items-center justify-between mb-10">
        <h1 class="text-4xl">Summary</h1>
        <div>
            <USelectMenu :items="transactionViewOptions" v-model="selectedView" />
        </div>
    </section>

    <section class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 sm:gap-16 mb-10">
        <Trend color="green" title="Income" :amount="incomeTotal" :last-amount="prevIncomeTotal" :loading="pending" />
        <Trend color="red" title="Expense" :amount="expenseTotal" :last-amount="prevExpenseTotal" :loading="pending" />
        <Trend color="green" title="Investments" :amount="4000" :last-amount="3000" :loading="pending" />
        <Trend color="red" title="Saves" :amount="4000" :last-amount="4200" :loading="pending" />
    </section>

    <section class="flex justify-between mb-10">
        <div>
            <h2 class="text-2xl font-extrabold">Transactions</h2>
            <div class="text-gray-500 dark:text-gray-400">
                You have {{ incomeCount }} incomes and {{ expenseCount }} expenses this period
            </div>
        </div>
        <div>
            <TransactionModal v-model="isOpen" @saved="refresh()" />
            <UButton icon="i-lucide-plus-circle" color="white" variant="solid" label="Add" @click="isOpen = true" />
        </div>
    </section>


    <section v-if="!pending">
        <div v-for="(transactionsOnDay, date) in byDate" :key="date" class="mb-10">
            <DailyTransactionSummary :date="date" :transactions="transactionsOnDay" />
            <Transaction v-for="transaction in transactionsOnDay" :key="transaction.id" :transaction="transaction"
                @deleted="refresh()" />

        </div>

    </section>
    <section v-else>
        <USkeleton class="h-8 w-full mb-2" v-for="i in 4" />
    </section>
</template>


<script setup>
import { useUseFetchTransaction } from '#imports';
import { transactionViewOptions } from '~/constants';

const selectedView = ref(transactionViewOptions[1])
const isOpen = ref(false)

const {current, previous} = useSelectedTimePeriod(selectedView)

const {pending, refresh, transaction: {
    incomeCount,
    expenseCount,
    incomeTotal,
    expenseTotal,
    grouped: {
        byDate
    }
}} = useUseFetchTransaction(current)

await refresh()

const {refresh: refreshPrevious, transactions: {
    incomeTotal: prevIncomeTotal,
    expenseTotal: prevExpenseTotal
} = {}} = useUseFetchTransaction(previous)
await refreshPrevious()

await refresh()

</script>