<script setup>
import Maintitle from "@/components/Header.vue";
import Balance from "@/components/Balance.vue";
import Expenses from "@/components/Expenses.vue";
import Transactions from "@/components/Transactions.vue";
import AddTransaction from "@/components/AddTransaction.vue";

import Footer from "./components/Footer.vue";
import { useToast } from "vue-toastification";

import { ref, computed, onMounted } from "vue";

const tost = useToast();

// const transactions = ref([
//   { id: 1, text: "Flowers", amount: -199.99 },
//   { id: 2, text: "Salary", amount: 2019.99 },
//   { id: 3, text: "Books", amount: -10.0 },
//   { id: 4, text: "Huggis", amount: 200.99 },
//   { id: 5, text: "Food", amount: 50.32 },
// ]);

const transactions = ref([]);

onMounted(() => {
  const savedTrasactions = JSON.parse(localStorage.getItem("transactions"));
  if (savedTrasactions) {
    transactions.value = savedTrasactions;
  }
});

//get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

//get income
const income = computed(() => {
  return transactions.value
    .filter((transactions) => transactions.amount > 0)
    .reduce((acc, transactions) => {
      return acc + transactions.amount;
    }, 0)
    .toFixed(2);
});

//get expense
const expense = computed(() => {
  return transactions.value
    .filter((transactions) => transactions.amount < 0)
    .reduce((acc, transactions) => {
      return acc + transactions.amount;
    }, 0)
    .toFixed(2);
});

//add transaction from form
const handleTrasactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });

  saveTransactionToLocalStorage();
  tost.success("Transaction added.");
};

const generateUniqueId = () => {
  return Math.floor(Math.random() * 100000);
};

const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(
    (transactions) => transactions.id !== id
  );
  saveTransactionToLocalStorage();
  tost.success("Trasaction deleted");
};

//saved to localstorage
const saveTransactionToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>

<template>
  <div class="maincontainer">
    <Maintitle />
    <div class="container">
      <Balance :total="total" />
    </div>
    <!-- adding + sign to convert -->
    <Expenses :income="+income" :expense="+expense" />

    <Transactions
      :transactions="transactions"
      @transactionDeleted="handleTransactionDeleted"
    />
    <AddTransaction @transactionSubitted="handleTrasactionSubmitted" />
    <Footer />
  </div>
</template>
