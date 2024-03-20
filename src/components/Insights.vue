<script setup lang="ts">
import axios from 'axios'
import { defineProps } from 'vue'

const { balance } = defineProps(['balance'])

const formatCurrency = (value: number) => {
  if (value) {
    return value.toLocaleString('pt-BR', {
      style: 'currency',
      currency: 'BRL'
    })
  }
}

const isPositive = (value: number) => {
  return value >= 0 ? 'green' : 'red'
}
</script>

<template>
  <div class="insights-container">
    <div class="insights-inner">
      <h2>Visão geral:</h2>
      <div class="insight-grid">
        <div>
          <span>Renda mensal:</span>
          <p class="blue" v-if="balance.monthlyIncome">
            {{ formatCurrency(balance.monthlyIncome) }}
          </p>
          <img v-else src="/empty.png" alt="" />
        </div>
        <div>
          <span>Despesas mensais:</span>
          <p class="golden" v-if="balance.monthlyExpense">
            {{ formatCurrency(balance.monthlyExpense) }}
          </p>
          <img v-else src="/empty.png" alt="" />
        </div>
        <div>
          <span>Saldo mensal:</span>
          <p :style="{ color: isPositive(balance.monthlyBalance) }" v-if="balance.monthlyBalance">
            {{ formatCurrency(balance.monthlyBalance) }}
          </p>
          <img v-else src="/empty.png" alt="" />
        </div>
        <div>
          <span>Saldo anual:</span>
          <p :style="{ color: isPositive(balance.yearlyBalance) }" v-if="balance.yearlyBalance">
            {{ formatCurrency(balance.yearlyBalance) }}
          </p>
          <img v-else src="/empty.png" alt="" />
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.insights-container {
  display: flex;
  height: 100%;
  padding: 5%;

  .insights-inner {
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

    div {
      display: flex;
      flex-direction: column;
      gap: 1rem;
      color: black;
    }
    .insight-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);

      div {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 0.4rem;
        background-color: #f7f7f7;
        border-radius: 0.5rem;
        border: 2px solid #9963f7;
        padding: 0.3rem;

        span {
          font-weight: bold;
          text-align: center;
        }

        p {
          font-size: 1.2rem;
        }

        img {
          height: 1.5rem;
        }

        .blue {
          color: #2077d4;
        }

        .golden {
          color: #f7b500;
        }
      }
    }
  }
}

@media (min-width: 1000px) {
  .insights-grid {
    grid-gap: 1.5rem 3rem;
    padding: 1rem 2rem;
  }
}
</style>
