<template>
  <Header /> 
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="income" :expenses="expenses"/>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
  </div>
</template>


<script setup>

  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';

  import { useToast } from 'vue-toastification';

  import { computed, ref, onMounted } from 'vue';

  const toast = useToast();

  const transactions = ref ([]);

  onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
    
    if(savedTransactions) {
      transactions.value = savedTransactions;
    }
  });

  // get total
  // console.log(transactions);
  // console.log(transactions.value);
  const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
  });
  console.log(typeof(total.value))

  // get income
  const income = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    Intl.NumberFormat('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 }).format(num);
  });
  console.log(typeof(income.value));

  // get expense
  const expenses = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount < 0 )
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    Intl.NumberFormat('en-US', {
      minimumFractionDigits: 2,
      maximumFractionDigits: 2 }) .format(num);
  });

  // add transaction
  const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      amount: transactionData.amount,
    });
    

    saveTransactionsToLocalStorage();

    toast.success('Transaction added');
    // console.log(generateUniqueId());
    
  }

  // Generate unique ID
  const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000);
  }

  // delete transcation
  const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id);

    saveTransactionsToLocalStorage();

    toast.success('Transaction deleted.')
    // console.log(id);
  }

  // Save to localStorage
  const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
  }
  
  // export default {
  //   components: {
  //     Header,
  //     Balance,
  //     IncomeExpenses,
  //     TransactionList,
  //     AddTransaction,
  //   },
  // };
</script>