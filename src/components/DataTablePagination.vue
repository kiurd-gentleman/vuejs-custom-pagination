<template>
  <div>
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
    {{ pageCount }}<br>
    {{ pageRange }}<br>
    {{ startPaginatorIndex }}<br>
    {{ endPaginatorIndex }}<br>
    {{ showFirstPageIndex }}<br>
    {{ showLastPageIndex }}<br>
    {{ showLessDots }}<br>
    {{ showMoreDots }}<br>
  </div>

</template>

<script>
export default {
  name: "DataTablePagination",
  props: {
    itemLength: {
      type: Number,
      required: true
    },
    itemPerPage: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
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
  watch: {
    itemLength: function () {
      this.pageCount = this.itemLength / this.itemPerPage
    },
    currentPage: function () {
      console.log('dataChange');
      this.$emit('dataChange', this.currentPage);
    }


  }
}
</script>

<style scoped>
.app {
  display: flex;
  justify-content: center;
}
</style>