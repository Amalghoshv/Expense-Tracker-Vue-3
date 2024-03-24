<template>
  <div>
    <Header/>
    <div class="container">
      <Balance :total="+total"/>
      <IncomeExpense :income="+income" :expenses="+expenses"/>
      <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" @trasactionEdited ="handleTransactionEdited"/>
      <AddTransactions @transactionSubmitted="handleTransactionSubmitted"/>
    </div>
  </div>
</template>
<script setup>
import AddTransactions from './components/AddTransactions.vue';
import Balance from './components/Balance.vue';
import Header from './components/Header.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import { useToast } from 'vue-toastification'
import { ref,computed ,onMounted } from 'vue';
const toast = useToast()
const transactions = ref([]);
onMounted(()=>{
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))
  if(savedTransactions){
    transactions.value = savedTransactions     
  }
})
//totals
const total = computed(()=>{
  return transactions.value.reduce((acc,transaction)=>{
      return acc+transaction.amount
  },0)
});
//income
const income = computed(()=>{
  return transactions.value.filter((transaction)=>transaction.amount > 0).
  reduce((acc,transaction)=>{
      return acc+transaction.amount
  },0).toFixed(2)
});
//expenses
const expenses = computed(()=>{
  return transactions.value.filter((transaction)=>transaction.amount < 0).
  reduce((acc,transaction)=>{
      return acc+transaction.amount
  },0).toFixed(2)
});
//Add Transaction
const handleTransactionSubmitted = (transactionData)=>{
  transactions.value.push({
    id:generateUniqueId(),
    text:transactionData.text,
    amount:transactionData.amount
  })
  saveTransactionsToLocalStorage();
  toast.success('New Transaction Added')
}
//generate id
const generateUniqueId = ()=>{
  return Math.floor(Math.random() * 1000000);
}
//delete Transaction
const handleTransactionDeleted = (id)=>{
   transactions.value = transactions.value.filter((transaction)=>transaction.id !== id);
   saveTransactionsToLocalStorage();
   toast.success('Transaction Deleted!')
}
//edit transaction
const handleTransactionEdited = (updatedTransaction) => {
  const index = transactions.value.findIndex((transaction) => transaction.id === updatedTransaction.id);
  if (index !== -1) {
    transactions.value[index] = updatedTransaction;
    saveTransactionsToLocalStorage();
    toast.success('Transaction Updated!');
  }
};

//save to localstorage
const saveTransactionsToLocalStorage = () =>{
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>