<template>
    <section class="bg-gray-900 w-1/3 mx-auto mt-10 p-2 px-5 rounded-md shadow-2xl">
      <!-- FAtura Bilgileri -->
          <AppHeading title="FaturaBilgileri" />        
      <ContactSection :contact="state.contact" />
          {{state.contact}}
      <div class="mt-5">
          <AppHeading title="Fatura Kalemleri"/>            
            <InvoiceItems :items="state.items" :AddInvoiceItem="AddInvoiceItem"/>
      </div>
      <!-- Summary -->
            <InvoiceSummary />


      <hr class="bg-gradient-to-r h-[1px] border-none from-gray-700 mt-5" />
      <div class="actionbar text-right my-5">
        <button class="purple-button">Kaydet</button>
      </div>
    </section>
</template>

<script setup>
import ContactSection from './ContactSection.vue';
import AppHeading from './ui/appHeading.vue';
import InvoiceItems from './invoiceItems.vue';
import InvoiceSummary from './invoiceSummary.vue';

import { reactive } from '@vue/reactivity';
const state = reactive({
  contact:{
    name:null,
    email:null,
    city:null,
    country:null,
    zipcode:null,
  },
    items:[],
  
})
const AddInvoiceItem = () => {
  state.items.push({
    name:null,
    qty:null,
    unit_price:0,
    total_price:0.0,
  })
}


</script>