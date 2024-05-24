<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList
      :transactions="transactions"
      @transactionDeletion="handelTransactionDeletion"
    />
    <AddTransaction @transactionSubmitted="handelTransactionSubmitted" />
  </div>
</template>
<script setup>
const toast = useToast();
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { toRef, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";
const transactions = toRef([]);
const total = computed(() => {
  return transactions.value.reduce((acc, trancation) => {
    return acc + trancation.amount;
  }, 0);
});
const income = computed(() => {
  return transactions.value
    .filter((trancation) => trancation.amount > 0)
    .reduce((acc, trancation) => {
      return acc + trancation.amount;
    }, 0)
    .toFixed(2);
});
const expenses = computed(() => {
  return transactions.value
    .filter((trancation) => trancation.amount < 0)
    .reduce((acc, trancation) => {
      return acc + trancation.amount;
    }, 0)
    .toFixed(2);
});
onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));
  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});
const handelTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  savedTransactionsToLocalStorage();

  toast.success("Transaction Added Successfully");
};
const handelTransactionDeletion = (id) => {
  transactions.value = transactions.value.filter(
    (trancation) => trancation.id !== id
  );
  savedTransactionsToLocalStorage();
  toast.success("Transaction Deleted Successfully");
};
const generateId = () => {
  return Math.floor(Math.random() * 1000000);
};
const savedTransactionsToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>
