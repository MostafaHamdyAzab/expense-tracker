<template>
  <div>
    <h3>History</h3>
    <ul id="list" class="list">
      <li
        v-for="transaction in transactions"
        :key="transaction.id"
        :class="transaction.amount < 0 ? 'minus' : 'plus'"
      >
        {{ transaction.text }}<span>${{ transaction.amount }}</span>
        <button @click="deleteTransaction(transaction.id)" class="delete-btn">
          X
        </button>
      </li>
    </ul>
  </div>
</template>
<script setup>
import { defineProps } from "vue";
const emit = defineEmits(["transactionDeletion"]);
const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});
const deleteTransaction = (id) => {
  emit("transactionDeletion", id);
};
</script>
