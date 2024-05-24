<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="text"
        id="amount"
        v-model="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
<script setup>
import { useToast } from "vue-toastification";
import { ref } from "vue";
const emit = defineEmits(["transactionSubmitted"]);
const toast = useToast();
const text = ref("");
const amount = ref("");
const onSubmit = () => {
  // console.log("Submit", text.value, amount.value);
  if (!text.value || !amount.value) {
    toast.error("Both Field Must Be Filled");
  }
  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value),
  };
  emit("transactionSubmitted", transactionData);
};
</script>
