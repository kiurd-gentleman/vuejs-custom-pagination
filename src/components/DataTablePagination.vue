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
        <li><a class="btn border" @click="currentPage -= 1">Previous</a></li>
        <li v-if="showFirstPageIndex"><a class="btn border" @click="currentPage = 1">1</a></li>
        <li v-if="showLessDots"><a class="btn border" @click="currentPage = startPaginatorIndex - 1">...</a></li>
        <li v-for="n in pageRange" :key="n" :class="{active : n == currentPage}"><a class="btn border" @click="currentPage = n">{{n}}</a></li>
        <li v-if="showMoreDots"><a class="btn border" @click="currentPage = endPaginatorIndex + 1">...</a></li>
        <li v-if="showLastPageIndex" @click="currentPage = pageCount"><a class="btn border">{{this.pageCount}}</a></li>
        <li><a class="btn border" @click="currentPage += 1">Next</a></li>
      </ul>
<!--      <ul class="pagination">-->
<!--        <li><a class="btn border">Previous</a></li>-->
<!--        <li><a class="btn border">1</a></li>-->
<!--        <li><a class="btn border">2</a></li>-->
<!--        <li><a class="btn border btn-info">3</a></li>-->
<!--        <li><a class="btn border">...</a></li>-->
<!--        <li><a class="btn border">9</a></li>-->
<!--        <li><a class="btn border">10</a></li>-->
<!--        <li><a class="btn border">11</a></li>-->
<!--        <li><a class="btn border">Next</a></li>-->
<!--      </ul>-->
    </div>
    {{currentPage}} <br>
    {{pageCount}}<br>
    {{pageRange}}<br>
    {{startPaginatorIndex}}<br>
    {{endPaginatorIndex}}<br>
    {{showFirstPageIndex}}<br>
    {{showLastPageIndex}}<br>
    {{showLessDots}}<br>
    {{showMoreDots}}<br>
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
  methods:{
    range(start, end){
      var ans = [];
      for (let i = start; i <= end; i++) {
        ans.push(i);
      }
      return ans;
    }
  },
  watch:{
    itemLength:function() {
     this.pageCount = this.itemLength / 10
    },

  }
}
</script>

<style scoped>
.app
{
  display: flex;
  justify-content: center;
}
</style>