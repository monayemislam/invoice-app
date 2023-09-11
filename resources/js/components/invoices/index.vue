<script setup>
import axios from "axios";
import { onMounted, ref } from "vue";

let invoices = ref([]);
let searchInvoice = ref([]);

onMounted(async () => {
  getInvoices();
});

const getInvoices = async () => {
  try {
    const response = await axios.get("/api/get_all_invoice");
    invoices.value = response.data.invoices; // Update the invoices data
    // console.log("Response", response);
  } catch (error) {
    console.error("Error fetching invoices", error);
  }
};

const search = async () => {
  let response = await axios.get(
    "/api/search_invoice?s=" + searchInvoice.value
  );
  // console.log("response", response.data.invoices);
  invoices.value = response.data.invoices;
};
</script>

<template>
  <div class="container">
    <!--==================== INVOICE LIST ====================-->
    <div class="invoices">
      <div class="card__header">
        <div>
          <h2 class="invoice__title">Invoices</h2>
        </div>
        <div>
          <a class="btn btn-secondary"> New Invoice </a>
        </div>
      </div>

      <div class="table card__content">
        <div class="table--filter">
          <span class="table--filter--collapseBtn">
            <i class="fas fa-ellipsis-h"></i>
          </span>
          <div class="table--filter--listWrapper">
            <ul class="table--filter--list">
              <li>
                <p class="table--filter--link table--filter--link--active">
                  All
                </p>
              </li>
              <li>
                <p class="table--filter--link">Paid</p>
              </li>
            </ul>
          </div>
        </div>

        <div class="table--search">
          <div class="table--search--wrapper">
            <select class="table--search--select" name="" id="">
              <option value="">Filter</option>
            </select>
          </div>
          <div class="relative">
            <i class="table--search--input--icon fas fa-search"></i>
            <input
              class="table--search--input"
              type="text"
              placeholder="Search invoice"
              v-model="searchInvoice"
              v-on:keyup="search()"
            />
          </div>
        </div>

        <div class="table--heading">
          <p>ID</p>
          <p>Date</p>
          <p>Number</p>
          <p>Customer</p>
          <p>Due Date</p>
          <p>Total</p>
        </div>

        <!-- item 1 -->
        <div v-if="invoices.length > 0">
          <div class="table--items" v-for="item in invoices" :key="item.id">
            <a href="#" class="table--items--transactionId"
              >#{{ item.id ? item.id : "" }}</a
            >
            <p>{{ item.date ? item.date : "" }}</p>
            <p>{{ item.number ? item.number : "" }}</p>
            <p>{{ item.customer ? item.customer.firstname : "" }}</p>
            <p>{{ item.due_date ? item.due_date : "" }}</p>
            <p>$ {{ item.total ? item.total : "" }}</p>
          </div>
        </div>
        <div v-else><div class="table--items">Invoice Not Found</div></div>
      </div>
    </div>
  </div>
</template>