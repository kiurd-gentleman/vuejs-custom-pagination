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
          <tr v-for="(singleData , index) in tableData" :key="index">
            <th scope="row" v-for="(header , headerIndex) in headers" :key="headerIndex">{{singleData[header.value]}}</th>
          </tr>
          </tbody>
        </table>
        <div>
          <ul v-if="pageCount > 1" class="pagination">
            <li><a class="btn border" @click="handlePreviousButton">Previous</a></li>
            <li v-if="showFirstPageIndex"><a class="btn border" @click="this.currentPage = 1">1</a></li>
            <li v-if="showLessDots"><a class="btn border" @click="this.currentPage = this.startPaginatorIndex - 1">...</a>
            </li>
            <li v-for="n in pageRange" :key="n" class="">
              <a class="btn border" :class="[currentPage === n ? active : '',]"
                 @click="this.currentPage = n">
                {{ n }}
              </a>
            </li>
            <li v-if="showMoreDots"><a class="btn border" @click="this.currentPage = this.endPaginatorIndex + 1">...</a>
            </li>
            <li v-if="showLastPageIndex" @click="this.currentPage = this.pageCount"><a class="btn border">{{
                this.pageCount
              }}</a></li>
            <li><a class="btn border" @click="handelNextButton">Next</a></li>
          </ul>
        </div>
        {{ currentPage }} <br>
        {{ pageCount }}xbgxcvb<br>
        {{ pageRange }}<br>
        {{ startPaginatorIndex }}<br>
        {{ endPaginatorIndex }}<br>
        {{ showFirstPageIndex }}<br>
        {{ showLastPageIndex }}<br>
        {{ showLessDots }}<br>
        {{ showMoreDots }}<br>
        <select  v-model="itemPerPage" @change="getPageItem">
          <option value="10">10</option>
          <option value="20">20</option>
          <option value="30">30</option>
          <option value="40">40</option>
          <option value="50">50</option>
        </select>
      </div>
  </div>

</template>

<script>
// import loader from "@/assets/loader.gif";

export default {
  name: "DataTableComponent",
  props: {
    headers: {
      type: Array,
      required: true
    },
    tableData: {
      type: Array,
      required: true
    },
    itemLength: {
      type: Number,
      required: true
    },
  },
  components: {},
  data(){
    return{
      data:[],
      skip: 0,
      is_loading: false,
      itemPerPage: 10,
      currentPage: 1,
      pageCount: '',
      active: 'btn-info'
    }
  },
  computed: {
    pageRange() {
      let start = this.startPaginatorIndex;
      let end = this.endPaginatorIndex;
      return this.range(start, end);
    },
    startPaginatorIndex() {
      let start = this.currentPage - 2;
      return (start > 0) ? start : 1;
    },
    endPaginatorIndex() {
      let end = this.currentPage + 2;
      return (end < this.pageCount) ? end : this.pageCount;
    },
    showFirstPageIndex() {
      return 1 < this.startPaginatorIndex;
    },
    showLastPageIndex() {
      return this.endPaginatorIndex < this.pageCount;
    },
    showLessDots() {
      return 2 < this.startPaginatorIndex;
    },
    showMoreDots() {
      return this.endPaginatorIndex < (this.pageCount - 1);
    },
  },
  methods: {
    getPageItem(){
      console.log(this.itemPerPage);
    },
    dataChange(value){
      this.skip = (value * 10) - 10;
      console.log(value , 'dataChange');
    },
    range(start, end) {
      var ans = [];
      for (let i = start; i <= end; i++) {
        ans.push(i);
      }
      return ans;
    },
    handlePreviousButton() {
      if (this.currentPage > 1) {
        this.currentPage -= 1;
      }
    },
    handelNextButton() {
      if (this.currentPage < this.pageCount) {
        this.currentPage += 1;
      }
    },
  },
  mounted() {
    // this.getData();
  },
  watch: {
    skip: function () {
      this.getData();
    },
    itemLength: function () {
      this.pageCount = this.itemLength / this.itemPerPage
      console.log(this.itemLength, 'itemLength');
      console.log(this.itemPerPage , 'itemPerPage');

    },
    currentPage: function () {
      console.log('dataChange');
      this.$emit('dataChange', this.currentPage);
    }
  }
}
</script>

<style scoped>


</style>