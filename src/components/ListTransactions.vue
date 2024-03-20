<script setup lang="ts">
import TransactionCard from '@/components/TransactionCard.vue'
import EmptyWarn from '@/components/EmptyWarn.vue'

const { transactions } = defineProps(['transactions'])

const emit = defineEmits(['transactionDeleted'])

const moveDeleteEvent = (id: number) => {
  emit('transactionDeleted', id)
}
</script>

<template>
  <div class="list-container">
    <div class="list-inner">
      <h2>Transactions</h2>
      <ul>
        <template v-if="transactions.length > 0">
          <TransactionCard
            v-for="item in transactions"
            :key="item.id"
            :transaction="item"
            @transactionDeleted="(id) => moveDeleteEvent(id)"
          />
        </template>
        <EmptyWarn v-else />
      </ul>
    </div>
  </div>
</template>

<style>
.list-container {
  display: flex;
  justify-content: center;
  height: 100%;
  padding: 5%;

  .list-inner {
    display: flex;
    border: 2px solid #9963f7;
    border-radius: 1rem;
    background-color: #f7f7f7;
    flex-direction: column;
    gap: 1rem;
    width: 100%;
    padding: 5%;
    h2 {
      color: black;
    }

    ul {
      max-height: 86.5vh;
      overflow-y: auto;
    }
  }
}
</style>
