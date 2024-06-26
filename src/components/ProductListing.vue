<script setup>
import ProductTable from './ProductTable.vue'
import axios from 'axios'
import { ref } from 'vue'

const uploadedFile = ref(null)
const productTable = ref(null)
const showResponse = ref(false)
const uploadResponse = ref('')

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
  }).then((response) => {
    uploadResponse.value = response.data.message
    showResponse.value = true
    productTable.value.getProducts()
  })
}
</script>

<template>
  <div class="mb-3 mx-1 row">
    <form action="" class="form-control input-group" @submit.prevent="false">
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
      <div class="col-md-2">
        <button class="input-group-text mx-2 btn btn-primary w-20" @click="handleUploadFile">
          Upload
        </button>
      </div>
      <div class="col-md-4" v-if="showResponse">
        <div class="alert alert-success alert-dismissible mb-0" role="alert">
          <div class="d-flex">
            <div>
              <!-- Download SVG icon from http://tabler-icons.io/i/check -->
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="icon alert-icon"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                stroke-width="2"
                stroke="currentColor"
                fill="none"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
                <path d="M5 12l5 5l10 -10"></path>
              </svg>
            </div>
            <div>{{ uploadResponse }}</div>
          </div>
          <a
            class="btn-close"
            data-bs-dismiss="alert"
            aria-label="close"
            @click="showResponse = false"
          ></a>
        </div>
      </div>
    </form>
  </div>
  <ProductTable ref="productTable" />
</template>
