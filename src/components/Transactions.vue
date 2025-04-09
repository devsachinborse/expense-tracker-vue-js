<script setup>
import { defineProps } from "vue";

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["transactionDeleted"]);

const deleteTrasaction = (id) => {
  emit("transactionDeleted", id);
};
</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
      v-for="transaction in transactions"
      :key="transaction.id"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span>
      <button @click="deleteTrasaction(transaction.id)" class="delete-btn">
        X
      </button>
    </li>

    <!-- <li class="minus">
      Grosory <span>-$400</span><button class="delete-btn">x</button>
    </li> -->
  </ul>
</template>
