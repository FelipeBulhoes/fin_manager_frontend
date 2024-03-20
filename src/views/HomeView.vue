<script setup lang="ts">
import AppHeader from '@/components/AppHeader.vue'
import CreateTransaction from '@/components/CreateTransaction.vue'
import ListTransactions from '@/components/ListTransactions.vue'
import BalanceInsights from '@/components/BalanceInsights.vue'
import { ref, onMounted } from 'vue'
import type { Ref } from 'vue'
import axios from 'axios'

export interface iTransaction {
  id: number
  title: string
  amount: number
  type: string
  periodicity: string
  description: string
  user: string
  created_at: string
  updated_at: string
}

export interface iBalance {
  monthlyIncome: number
  monthlyExpense: number
  monthlyBalance: number
  yearlyBalance: number
}

const transactions: Ref<iTransaction[]> = ref([])
const balance: Ref<iBalance> = ref({} as iBalance)

const insertNewTransaction = async (transaction: any) => {
  transactions.value.push(transaction)
  await getBalance()
}

const removeTransaction = async (id: number) => {
  transactions.value = transactions.value.filter((transaction: any) => transaction.id !== id)
  await getBalance()
}

const getBalance = async () => {
  const response = await axios.get('http://localhost:8000/transactions/balance/', {
    headers: {
      Authorization: `Bearer ${localStorage.getItem('token')}`
    }
  })
  console.log(response.data)

  balance.value = response.data
}

onMounted(async () => {
  const authtoken = localStorage.getItem('token')

  if (!authtoken) {
    window.location.href = '/login'
  }

  await axios
    .get('http://localhost:8000/transactions/list/', {
      headers: {
        Authorization: `Bearer ${authtoken}`
      }
    })
    .then((response) => {
      transactions.value = response.data
    })
    .catch((error) => {
      console.log(error)
    })
  await getBalance()
})
</script>

<template>
  <AppHeader />
  <main>
    <section>
      <CreateTransaction
        @transactionCreated="(newTransaction: any) => insertNewTransaction(newTransaction)"
      />
    </section>
    <section>
      <ListTransactions
        :transactions="transactions"
        @transactionDeleted="(id: number) => removeTransaction(id)"
      />
    </section>
    <section>
      <BalanceInsights :balance="balance" />
    </section>
  </main>
</template>

<style>
main {
  width: 100%;
  min-height: 86.5vh;
  padding: 10vh 5vw 0 5vw;

  @media (min-width: 1000px) {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: repeat(2, 1fr);

    section:nth-child(1) {
      grid-column: 1;
      grid-row: 1;
    }

    section:nth-child(2) {
      grid-column: 2;
      grid-row-start: 1;
      grid-row-end: 3;
    }

    section:nth-child(3) {
      grid-column: 1;
      grid-row: 2;
    }
  }

  @media (min-width: 1366px) {
    padding: 9vh 10vw 0 10vw;
  }

  @media (min-width: 1600px) {
    padding: 9vh 14vw 0 14vw;
  }

  @media (min-width: 1921px) {
    padding: 9vh 20vw 0 20vw;
  }
}
</style>
