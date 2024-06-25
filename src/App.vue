<script setup>
import ProductSyncForm from './components/ProductSyncForm.vue'
import ProductTable from './components/ProductTable.vue'
</script>

<template>
  <main>
    <div class="page-wrapper">
      <!-- Page header -->
      <div class="page-header d-print-none">
        <div class="container-xl">
          <div class="row g-2 align-items-center">
            <div class="col">
              <h1 class="page-title">Product Listing</h1>
            </div>
          </div>
        </div>
      </div>
      <!-- Page body -->
      <div class="page-body">
        <div class="container-xl">
          <!-- Content here -->
          <ProductSyncForm />
          <ProductTable />
        </div>
      </div>
    </div>
  </main>
</template>
