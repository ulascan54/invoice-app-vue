<template>
    <section class="bg-gray-900 w-1/3 mx-auto mt-10 p-2 px-5 rounded-md shadow-2xl">
      <!-- FAtura Bilgileri -->
          <AppHeading title="FaturaBilgileri" />        
      <ContactSection :contact="state.contact" />
      <div class="mt-5">
          <AppHeading title="Fatura Kalemleri"/>            
            <InvoiceItems :items="state.items" :AddInvoiceItem="AddInvoiceItem"/>
      </div>
      <!-- Summary -->
            <InvoiceSummary :items="state.items" />


      <hr class="bg-gradient-to-r h-[1px] border-none from-gray-700 mt-5" />
      <div class="actionbar text-right my-5">
        <button class="purple-button" @click="onSubmit">Kaydet</button>
      </div>
    </section>
</template>

<script setup>
import ContactSection from './ContactSection.vue';
import AppHeading from './ui/appHeading.vue';
import InvoiceItems from './invoiceItems.vue';
import InvoiceSummary from './invoiceSummary.vue';
import { provide, watch } from "@vue/runtime-core";
import { reactive} from '@vue/reactivity';
const props = defineProps({saveInvoice:Function,activeInvoice:[Object,null]})
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
    id: new Date().getTime(),
    name:null,
    qty:null,
    unit_price:0,
    total_price:0.0,
  })
}

const DeleteInvoiceItem = (invoiceItem)=>{
    state.items=state.items.filter(f=>f.id !== invoiceItem.id)
}
provide('DeleteInvoiceItem',DeleteInvoiceItem)
const onSubmit = ()=>{
  props.saveInvoice({...state, created_at: new Date().getTime(), id: new Date().getTime()})
  state.contact= {
    name:null,
    email:null,
    city:null,
    country:null,
    zipcode:null,
  }
  state.items=[]
}
  console.log(props.activeInvoice);

watch(()=>props.activeInvoice,(activeInvoice)=>{
      if (activeInvoice!==null) {
        state.contact={
          ...activeInvoice.contact
        }
        state.items=[...activeInvoice.items]
      }
})
</script>