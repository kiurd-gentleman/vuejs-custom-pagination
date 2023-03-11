<template>
  <div>
    <table class="table table-bordered">
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
    <DataTablePagination :itemLength="itemLength"></DataTablePagination>
  </div>

</template>

<script>
import axios from "axios";
import DataTablePagination from "@/components/DataTablePagination";

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
    }
  },
  methods: {
    pagination(){
      console.log('pagination');

    },
    functionName(pageNumber){
      console.log(pageNumber);
    }
  },
  mounted() {
    axios.get('https://dummyjson.com/users?limit=50&skip=0')
      .then(response => {
        this.data = response.data.users;
        this.itemLength = response.data.users.length;
        console.log(response);
      })
      .catch(error => {
        console.log(error);
      });
  }
}
</script>

<style scoped>


</style>