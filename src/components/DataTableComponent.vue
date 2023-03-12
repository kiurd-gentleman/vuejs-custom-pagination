<template>
  <div>
      <div >

      </div>
      <div >
        <img v-if="is_loading === true"  src="@/assets/loader2.gif" alt="loader">
        <table v-else class="table table-bordered">
          <thead>
          <tr>
            <th scope="col" v-for="(header , index) in headers" :key="index">{{ header.text }}</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="(singleData , index) in data" :key="index">
            <th scope="row" v-for="(header , headerIndex) in headers" :key="headerIndex">{{singleData[header.value]}}</th>
          </tr>
          </tbody>
        </table>
        <DataTablePagination :itemLength="itemLength" @dataChange="dataChange"></DataTablePagination>
      </div>
  </div>

</template>

<script>
import axios from "axios";
import DataTablePagination from "@/components/DataTablePagination";
// import loader from "@/assets/loader.gif";

export default {
  name: "DataTableComponent",
  props: {
    headers: {
      type: Array,
      required: true
    }
  },
  components: {DataTablePagination},
  data(){
    return{
      data:[],
      itemLength: 10,
      skip: 0,
      is_loading: false,
    }
  },
  methods: {
    pagination(){
      console.log('pagination');

    },
    dataChange(value){
      this.skip = (value * 10) - 10;
      console.log(value , 'dataChange');
    },
    getData(){
      this.is_loading = true;
      axios.get('https://dummyjson.com/users?limit=10&skip=' + this.skip)
          .then(response => {
            this.data = response.data.users;
            this.itemLength = 100;
            console.log(response);
            this.is_loading = false;
          })
          .catch(error => {
            console.log(error);
          });
    }
  },
  mounted() {
    this.getData();
  },
  watch: {
    skip: function () {
      this.getData();
    }
  }
}
</script>

<style scoped>


</style>