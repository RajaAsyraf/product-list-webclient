<script setup>
import { ref, onMounted, watch } from 'vue'
import axios from 'axios'
import { Bootstrap5Pagination } from 'laravel-vue-pagination'

const products = ref([])
const pagination = ref([])
const perPage = ref(10)

watch(perPage, () => {
  getProducts()
})

const getProducts = async (page = 1) => {
  try {
    let response = await axios.get(
      `http://localhost/api/product?page=${page}&perPage=${perPage.value}`
    )
    pagination.value = response.data
    products.value = pagination.value.data
  } catch (error) {
    console.error('Error fetching data:', error)
  }
}

onMounted(getProducts)
</script>

<template>
  <div class="col-12">
    <div class="card">
      <div class="card-header">
        <h3 class="card-title">Products</h3>
      </div>
      <div class="card-body border-bottom py-3">
        <div class="d-flex">
          <div class="text-secondary">
            Show
            <div class="mx-2 d-inline-block">
              <input
                type="number"
                class="form-control form-control-sm"
                v-model="perPage"
                size="3"
                aria-label="Product per page"
              />
            </div>
            entries
          </div>
          <div class="ms-auto text-secondary">
            Search:
            <div class="ms-2 d-inline-block">
              <input type="text" class="form-control form-control-sm" aria-label="Search invoice" />
            </div>
          </div>
        </div>
      </div>
      <div class="table-responsive">
        <table class="table card-table table-vcenter text-nowrap datatable">
          <thead>
            <tr>
              <th>No</th>
              <th class="w-1">
                Product ID
                <!-- Download SVG icon from http://tabler-icons.io/i/chevron-up -->
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="icon icon-sm icon-thick"
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
                  <path d="M6 15l6 -6l6 6"></path>
                </svg>
              </th>
              <th>Type</th>
              <th>Brand</th>
              <th>Model</th>
              <th>Capacity</th>
              <th>Quantity</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(product, index) in products" :key="index">
              <td>{{ product.id }}</td>
              <td>
                <span class="text-secondary">{{ product.product_id }}</span>
              </td>
              <td>{{ product.type }}</td>
              <td>{{ product.product_model.brand.name }}</td>
              <td>{{ product.product_model.name }}</td>
              <td>{{ product.capacity }}</td>
              <td>{{ product.quantity }}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="card-footer d-flex align-items-center">
        <p class="m-0 text-secondary">
          Showing <span>{{ pagination.from }}</span> to <span>{{ pagination.to }}</span> of
          <span>{{ pagination.total }}</span> entries
        </p>
        <ul class="pagination m-0 ms-auto">
          <Bootstrap5Pagination :data="pagination" @pagination-change-page="getProducts" />
        </ul>
      </div>
    </div>
  </div>
</template>
