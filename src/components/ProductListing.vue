<script setup>
import ProductTable from './ProductTable.vue'
import axios from 'axios'
import { ref } from 'vue'

const uploadedFile = ref(null)
const productTable = ref(null)

const handleUploadFile = async () => {
  if (uploadedFile.value.files[0] === undefined) {
    return
  }

  const formData = new FormData()
  formData.append('product_sync_file', uploadedFile.value.files[0])
  await axios({
    method: 'post',
    url: 'http://localhost/api/product-sync',
    data: formData,
    config: { headers: { 'Content-Type': 'multipart/form-data' } }
  }).then(() => {
    productTable.value.getProducts()
  })
}
</script>

<template>
  <div class="mb-3 row">
    <form action="" class="input-group" @submit.prevent="false">
      <div class="col-md-6">
        <input
          type="file"
          name="file"
          id="file"
          class="form-control"
          ref="uploadedFile"
          accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel"
          required
        />
      </div>
      <div class="col-md-4">
        <button class="input-group-text mx-2 btn btn-primary w-20" @click="handleUploadFile">
          Upload
        </button>
      </div>
    </form>
  </div>
  <ProductTable ref="productTable" />
</template>
