<template>
  <DataTableComponent :headers="headers" :tableData="tableData" :itemLength="itemLength"/>
</template>

<script>
import DataTableComponent from "@/components/DataTableComponent";
import axios from "axios";
export default {
  name: "DataTable",
  components: {DataTableComponent},
  data(){
    return{
      headers: [
        {text: '#', value: 'id'},
        {text: 'First Name', value: 'firstName'},
        {text: 'Last Name', value: 'lastName'},
        {text: 'MacAddress', value: 'macAddress'},
        {text: 'Email', value: 'email'},
        {text: 'Phone', value: 'phone'},
      ],
      tableData:[],
      itemLength: 10,
      skip: 0,
    }
  },
  methods: {
    getData(){
      this.is_loading = true;
      axios.get('https://dummyjson.com/users?limit=10&skip=' + this.skip)
          .then(response => {
            this.tableData = response.data.users;
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
  }
}
</script>

<style scoped>

</style>