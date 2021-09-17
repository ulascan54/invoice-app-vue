<template>
  <div class="w-screen h-screen bg-gray-800 flex flex-row text-white items-start justify-center">
    <AppSidebar />
    <InvoiceContent />
  </div>
</template>
<script setup>
import AppSidebar from './components/AppSidebar.vue';
import InvoiceContent from './components/InvoiceContent.vue';


</script>