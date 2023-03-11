<template>
  <div>
    <!--    <div class="btn-group">-->
    <!--      <button class="btn border">Previous</button>-->
    <!--      <button class="btn border">1</button>-->
    <!--      <button class="btn border btn-info">2</button>-->
    <!--      <button class="btn border">3</button>-->
    <!--      <button class="btn border disabled">...</button>-->
    <!--      <button class="btn border">10</button>-->
    <!--      <button class="btn border">11</button>-->
    <!--      <button class="btn border">12</button>-->
    <!--      <button class="btn border">Next</button>-->
    <!--    </div>-->
    <div>
      <ul v-if="pageCount > 1" class="pagination">
        <li><a class="btn border" @click="handlePreviousButton">Previous</a></li>
        <li v-if="showFirstPageIndex"><a class="btn border" @click="handelFirstButton">1</a></li>
        <li v-if="showLessDots"><a class="btn border" @click="handelFirstDotsButton">...</a></li>
        <li v-for="n in pageRange" :key="n" class=""><a class="btn border" @click="handelMiddleButton(n)">{{ n }}</a>
        </li>
        <li v-if="showMoreDots"><a class="btn border" @click="handelSecondDotsButton">...</a></li>
        <li v-if="showLastPageIndex" @click="handelLastButton"><a class="btn border">{{ this.pageCount }}</a></li>
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
    }
  },
  data() {
    return {
      currentPage: 1,
      pageCount: '',
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
    }
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
      console.log('dataChange');
    },
    handelNextButton() {
      if (this.currentPage < this.pageCount) {
        this.currentPage += 1;
      }
      console.log('dataChange');
    },
    handelFirstButton() {
      this.currentPage = 1;
    },
    handelFirstDotsButton() {
      this.currentPage = this.startPaginatorIndex - 1;
    },
    handelMiddleButton(n) {
      this.currentPage = n
    },
    handelSecondDotsButton() {
      this.currentPage = this.endPaginatorIndex + 1;
    },
    handelLastButton() {
      this.currentPage = this.pageCount;
    }
  },
  watch: {
    itemLength: function () {
      this.pageCount = this.itemLength / 10
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