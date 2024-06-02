<template>
    <h3>
        Add new transaction
    </h3>
    <form 
        @submit.prevent="onsubmit"
        id="form" 
        action="">
        <div class="form-control">
            <input 
                v-model="text"
                type="text"
                id="text"
                placeholder="Enter text..."
            >
        </div>
        <div class="form-control">
            <label for="amount">
                Amount <br/>
                (negative - expense, positive - income)
            </label>
            <input 
                v-model="amount"
                type="number"
                id="amount"
                step='0.01'
                value='0.00'
                placeholder="Enter amount..."
            >
        </div>
        <button class="btn">
            Add transaction 
        </button>
    </form>
</template>

<script setup>
    import {ref} from 'vue';
    import {useToast} from 'vue-toastification';

    const text = ref('');
    const amount = ref('');

    const emit = defineEmits(['transactionSubmitted']);

    const toast = useToast();

    const onsubmit = () => {
        if(!text.value|| !amount.value) {
            toast.error('Both fields must be filled');
            return;
        }
        console.log(text.value, amount.value);

        const transactionData = {
            text: text.value,
            // text: text.value,
            amount: parseFloat(amount.value),
        }
        emit('transactionSubmitted', transactionData);

        text.value = '';
        amount.value = '';
    };
</script>