

<template>
 
    <div class="container overflow-auto mt-5">

        <div id="pageD1" class="col-sm-3">
            <span>Show</span>
            <!-- @change="handlePageSizeChange($event)" -->
            <select v-model="perPage" @change="handlePageSizeChange($event)">
              <option v-for="size in pageSizes" :key="size" :value="size">{{ size }}</option>
            </select>
            
        </div>
      <div class="col-md-8 mt-5">
        <div class="input-group mb-3">
          <input
            type="text"
            class="form-control"
            placeholder="Search by title"
            v-model="searchTitle"
          />
          <div class="input-group-append">
            <button
              class="btn btn-outline-secondary"
              type="button"
              @click="page = 1; retrieveTutorials();"
            >
              Search
            </button>
          </div>
        </div>
      </div>
  
  
      
  
      <p class="mt-3">Current Page: {{ currentPage }}</p>
      <table id="table" class="table table table-bordered mt-5 border border-2">
      
        <thead>
          <tr>
            <th class="text-center">User ID</th>
            <th class="text-center">Admin Status</th>
            <th class="text-center">Order Status</th>
            <th class="text-center">Delivery Status</th>
          </tr>
        </thead>
        <tbody>
            <tr v-for="order in items" :key="order.id">
                <td class="TableBodyText">{{ order.user_id }}</td>
                <td class="TableBodyText">{{ order.admin_status }}</td>

                <td class="TableBodyText">{{order.order_status }}</td>
                <td class="TableBodyText">{{ order.delivery_status }}</td>
            </tr>
          
        </tbody>
      </table>
  
      <!-- <b-table
        id="my-table"
        :items="items"
        :per-page="perPage"
        :current-page="currentPage"
        small
      ></b-table> -->
      <!-- <b-pagination
        v-model="currentPage"
        :total-rows="rows"
        :per-page="perPage"
        aria-controls="my-table"
      ></b-pagination> -->
      <b-pagination v-model="currentPage" :total-rows="totalItems" :per-page="perPage" 
                                          first-text="First" prev-text="Prev" next-text="Next" last-text="Last">
                            </b-pagination>
    </div>
  </template>
  
  <script>
    import axios from 'axios'
    // import $ from 'jquery'

    export default {
      name: 'data-Table',
      data() {
        return {
          perPage: 5,
          currentPage: 1,
          pageSizes: [5, 15, 20, 30],
          items: [],
          totalItems: 0,
        }
      },
      mounted() {
        this.getOrderDataList();
      },
      methods:{
        async getOrderDataList() {
            await axios.get(`https://api.ambor.com.bd/ambor/Cart/admin_pending_orders/?page=${this.currentPage}&size=${this.perPage}`)
                .then(resp => {
                    this.items = resp.data.results;
                    this.totalItems = resp.data.count;
                })
        },
        handlePageSizeChange(event) {
            this.perPage = parseInt(event.target.value);
            this.getOrderDataList();
        },
      },
      computed: {
            rows() {
                return this.totalItems
            }
      },
      watch: {
            currentPage: {
                handler: function() {
                this.getOrderDataList();
                }
            }
        },
            
   
    }

  </script>