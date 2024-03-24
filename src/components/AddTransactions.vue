<template>
     <h3>Add new transaction</h3>
      <form id="form"  @submit.prevent="onSubmit()">
        <div class="form-control">
          <label for="text">Text</label>
          <input type="text" id="text" v-model="text" placeholder="Enter text..." />
        </div>
        <div class="form-control">
          <label for="amount"
            >Amount <br />
            (negative - expense, positive - income)</label
          >
          <input type="text" id="amount" class="input" v-model="amount" placeholder="Enter amount..." />
        </div>
        <button class="btn" style="font-weight:bold">Add transaction <svg xmlns="http://www.w3.org/2000/svg" width="1.5em" height="1.5em" viewBox="0 0 24 24"><path fill="currentColor" d="M22 5.18L10.59 16.6l-4.24-4.24l1.41-1.41l2.83 2.83l10-10zM12 20c-4.41 0-8-3.59-8-8s3.59-8 8-8c1.57 0 3.04.46 4.28 1.25l1.45-1.45A10.02 10.02 0 0 0 12 2C6.48 2 2 6.48 2 12s4.48 10 10 10c1.73 0 3.36-.44 4.78-1.22l-1.5-1.5c-1 .46-2.11.72-3.28.72m7-5h-3v2h3v3h2v-3h3v-2h-3v-3h-2z"/></svg></button>
      </form>
</template>
<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification'

const text = ref('')
const amount = ref('');
const emit = defineEmits('transactionSubmitted')
const toast = useToast()
const onSubmit =() =>{
    if(!text.value || !amount.value){
        toast.error('Both fields must be filled');
        return;
    }
    const transactionData = {
        text:text.value,
        amount:parseFloat(amount.value),
    };

    emit('transactionSubmitted' ,transactionData)
    text.value='';
    amount.value = '';
}

</script>
<style scoped>
svg{
    vertical-align:middle
}
.btn{
    border-radius:5% ;
    border:3px solid tan
}
.btn:hover{
    background-color: lightgreen;;
    border:3px solid tan;
    color:black;
   
}
</style>