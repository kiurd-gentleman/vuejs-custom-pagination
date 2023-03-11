<template>
  <DataTableComponent :headers="headers"/>
</template>

<script>
import DataTableComponent from "@/components/DataTableComponent";
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
    }
  }
}
</script>

<style scoped>

</style>