<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="+income" :expense="+expense"/>
    <TransactionList :transactions="transactions" @deleteTransaction="handleDeleteTransaction"  />
    <AddTransaction  @transactionSubmitted="handleTransactionSubmitted"/>

  </div>
</template>

<script setup>

import Header from "@/components/Header.vue";
import Balance from "@/components/Balance.vue";
import IncomeExpenses from "@/components/IncomeExpenses.vue";
import TransactionList from "@/components/TransactionList.vue";
import AddTransaction from "@/components/AddTransaction.vue";
import {computed, handleError, ref} from "vue";
import Toast, {useToast} from "vue-toastification";
const  toast=useToast();


const transactions = ref(
  [
    { id: 1, text: "Flower", amount: -20 },
    { id: 2, text: "Salary", amount: 100 },
    { id: 3, text: "Book", amount: -70 },
    { id: 4, text: "Camera", amount: 150 },
  ]
);

const total= computed(()=>{
  return transactions.value.reduce((acc,transaction)=>{return acc+transaction.amount;},0);
});

const income= computed(()=>{
  return transactions.value
      .filter(transaction=>transaction.amount>0)
      .reduce((acc,transaction)=>{return acc+transaction.amount;},0)
      .toFixed(2);
});

const  expense = computed(()=>{
   return transactions.value
       .filter(transaction=>transaction.amount<0)
       .reduce((acc,transaction)=>{
         return acc+transaction.amount;
           },0
       ).toFixed(2)
});

const handleTransactionSubmitted=(transactionData)=>{
  transactions.value.push({
    id:transactions.value.length+1,
    text:transactionData.text,
    amount:transactionData.amount
  });
}

const handleDeleteTransaction=(id)=>{
  transactions.value=transactions.value.filter(transaction=>transaction.id!==id);
  toast.success('Transaction deleted');

}
</script>

