<template>
 <b-row>
   
      <b-col md="6" class="my-1">
        <b-form-group horizontal label="Filter" class="mb-0">
          <b-input-group>
            <b-form-input v-model="filter" placeholder="Type to Search" />
            <b-input-group-append>
              <b-btn :disabled="!filter" @click="filter = ''">Clear</b-btn>
            </b-input-group-append>
          </b-input-group>
        </b-form-group>
      </b-col>

  <b-table striped hover :items="items" :fields="fields" :filter="filter" :per-page="pageSize" :current-page="pageIndex">
    <template slot="Detail" slot-scope="row">
      <!-- we use @click.stop here to prevent emitting of a 'row-clicked' event  -->
      <b-button size="sm" @click.stop="row.toggleDetails" class="mr-2">
       {{ row.detailsShowing ? 'Hide' : 'Show'}} Details
      </b-button>
      <!-- In some circumstances you may need to use @click.native.stop instead -->
      <!-- As row.showDetails is one-way, we call the toggleDetails function on @change -->
    </template>
    <template slot="row-details" slot-scope="row">
      <b-card style="background-color: pink;">
        <b-row class="mb-2">
          <b-col sm="3" class="text-sm-right"><b>freight:</b></b-col>
          <b-col>{{ row.item.freight }}</b-col>
        </b-row>
        <b-row class="mb-2">
          <b-col sm="3" class="text-sm-right"><b>ship_name:</b></b-col>
          <b-col>{{ row.item.ship_name }}</b-col>
        </b-row>
        <b-row class="mb-2">
          <b-col sm="3" class="text-sm-right"><b>ship_address:</b></b-col>
          <b-col>{{ row.item.ship_address }}</b-col>
        </b-row>
        <b-button size="sm" @click="row.toggleDetails">Hide Details</b-button>
      </b-card>
    </template>
  </b-table>
   <b-col md="6" class="my-1">
  <b-pagination align="center" size="md" :total-rows="items.length" v-model="pageIndex" :per-page="pageSize">
    </b-pagination> 
     </b-col>
    <br>
 <b-col md="6" class="my-1">
    <div>CurrentPage: {{pageIndex}}</div>  
     </b-col>
  </b-row>
</template>

<script>
import axios from "axios";
export default {
  data () {
    return {
      message: "Project 2",
      items: [],
      filter: null,
      pageSize:15,
      pageIndex:1,
      fields: [
        {
          key: "order_id",
          sortable: true
        },
        {
          key: "customer_id",
          sortable: true
        },
        {
          key: "ship_city",
          sortable: true,
          
        },
        {
          key: "ship_country",
          sortable: true, 
        },
        {
          key : 'Detail'
        }
      ]
    }
  },
  computed: {
    sortOptions () {
      // Create an options list from our fields
      return this.fields
        .filter(f => f.sortable)
        .map(f => { return { text: f.label, value: f.key } })
    }
  },
  async mounted() {
    let order = await axios.get(
      "https://nameless-reaches-35082.herokuapp.com/api/order"
    );
    order = order.data.data;
    this.items = order;
   
  }
}
 
  

</script>