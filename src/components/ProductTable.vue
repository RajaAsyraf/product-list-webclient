<script setup>
import { ref, onMounted, watch } from 'vue'
import axios from 'axios'
import { Bootstrap5Pagination } from 'laravel-vue-pagination'
import debounce from 'lodash.debounce'

const products = ref([])
const pagination = ref([])
const perPage = ref(10)
const searchValue = ref('')

watch(
  perPage,
  debounce(() => {
    getProducts()
  }, 500)
)

watch(
  searchValue,
  debounce(() => {
    getProducts()
  }, 500)
)

const getProducts = async (page = 1) => {
  try {
    let response = await axios.get(
      `http://localhost/api/product?page=${page}&perPage=${perPage.value}&search=${searchValue.value}`
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
              <input
                type="text"
                class="form-control form-control-sm"
                v-model="searchValue"
                aria-label="Search invoice"
              />
            </div>
          </div>
        </div>
      </div>
      <div class="table-responsive">
        <table class="table card-table table-vcenter text-nowrap datatable">
          <thead>
            <tr>
              <th>No</th>
              <th>Product ID</th>
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
