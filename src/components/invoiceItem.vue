<template>
    <div class="table-body-item group w-full">
      <input autocomplete="off" v-model="item.name" type="text" class="input w-4/12 mr-2" />
      <input autocomplete="off" v-model="item.qty" type="text" class="input w-2/12 mr-2" />
      <input autocomplete="off" v-model="item.unit_price" type="text" class="input w-2/12 mr-2" />
      <span class="p-1 text-center text-gray-400 w-3/12 overflow-hidden">${{item.total_price}}</span>
      <div class="text-right w-1/12">
        <button class="delete-button" @click="DeleteInvoiceItem(item)">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="fill-current"
            height="24"
            viewBox="0 0 24 24"
            width="24"
          >
            <path d="M0 0h24v24H0V0z" fill="none" />
            <path
              d="M16 9v10H8V9h8m-1.5-6h-5l-1 1H5v2h14V4h-3.5l-1-1zM18 7H6v12c0 1.1.9 2 2 2h8c1.1 0 2-.9 2-2V7z"
            />
          </svg>
        </button>
      </div>
    </div>
</template>
<script setup>
import { computed } from "@vue/reactivity";
import { inject, watch } from "@vue/runtime-core";

const props =  defineProps({item:Object})
const totalPrice = computed(()=> props.item.qty * props.item.unit_price)
const DeleteInvoiceItem = inject(['DeleteInvoiceItem'])
watch(totalPrice,(totalPrice)=>{
    props.item.total_price=totalPrice
})


</script>