<template>
  <div class="w-screen h-screen bg-gray-800 flex flex-row text-white items-start justify-center">
    <AppSidebar :invoices="invoiceList" :invoiceDetails="invoiceDetails"/>
    <InvoiceContent :saveInvoice="saveInvoice" :activeInvoice="state.activeInvoice" :detailsStatus="state.status" @status="status"/>
  </div>
</template>
<script setup>
import { reactive, ref } from '@vue/reactivity';
import AppSidebar from './components/AppSidebar.vue';
import InvoiceContent from './components/InvoiceContent.vue';
const invoiceList=ref([])
const state = reactive({activeInvoice:null , status:true})
const saveInvoice =(invoice)=>{
  invoiceList.value.push(invoice)
  console.log('invoice :>> ', invoice);
}
const status = ()=>{
  state.status=true
}
const invoiceDetails =(invoice)=>{
  state.activeInvoice=invoice;
  state.status=false;
  setTimeout(() => {
    state.activeInvoice=null
  }, 1000);
}
</script>