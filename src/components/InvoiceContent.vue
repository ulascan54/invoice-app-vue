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


      <hr  class="bg-gradient-to-r h-[1px] border-none from-gray-700 mt-5" />
      <div v-if="detailsStatus" class="actionbar text-right my-5">
        <button class="purple-button" @click="onSubmit">Kaydet</button>
      </div>
      <div v-else  class="actionbar text-right my-5">
        <button class="purple-button bg-red-700 hover:bg-red-900 mr-3" @click="clearBoard">Vazgeç</button>
        <button class="purple-button" @click="updateInvoice">Güncelle</button>
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
import db from '../boot/firebase'
const props = defineProps({activeInvoice:[Object,null],detailsStatus:Boolean})
const emit=defineEmits()
const state = reactive({
  contact:{
    name:null,
    email:null,
    city:null,
    country:null,
    zipcode:null,
  },
    items:[],
    created_at:null,
    id:null,
  
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

const clearBoard=()=>{
    state.contact= {
    name:null,
    email:null,
    city:null,
    country:null,
    zipcode:null,
  }
  state.items=[],
  state.created_at=null
  emit('status')
}

provide('DeleteInvoiceItem',DeleteInvoiceItem)
const onSubmit = ()=>{
  db.collection('invoices')
    .add({...state, created_at: new Date().getTime()})
    .then((docRef)=>{
      console.log('Database e yazdırma işlemi tamamlandı' ,docRef.id)
    })
    .catch((err)=>console.error("Database e yazdırma işlemi hatalı",err))

  state.contact= {
    name:null,
    email:null,
    city:null,
    country:null,
    zipcode:null,
  }
  state.items=[],
  state.created_at=null

}
const updateInvoice =()=>{
  db.collection("invoices").doc(state.id)
    .update({
      contact: {...state.contact}, 
      items:{...state.items}
      });
      clearBoard()
}

watch(()=>props.activeInvoice,(activeInvoice)=>{
      if (activeInvoice!==null) {
        state.contact={
          ...activeInvoice.contact
        }
        state.items=[...activeInvoice.items]
        state.created_at=activeInvoice.created_at
        state.id=activeInvoice.id
      }
})
</script>