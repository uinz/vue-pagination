<template>
  <div>
    <span class="page-item" @click="prevPage">👈</span>
    <span
      :class="{ 'page-item': true, active: item.page === current_ }"
      :key="item.page"
      v-for="item of items"
      @click="changePage(item.page)"
    >{{item.title}}</span>
    <span class="page-item" @click="nextPage">👉</span>
  </div>
</template>



<script>
export default {
  props: {
    pageSize: {
      type: Number,
      default: 10
    },
    current: {
      type: Number,
      default: 1
    },
    total: {
      type: Number,
      default: 200
    }
  },
  data() {
    return {
      current_: this.current,
      pageSize_: this.pageSize
    };
  },
  computed: {
    totalPage() {
      return Math.ceil(this.total / this.pageSize);
    },
    items() {
      const items = Array(this.totalPage)
        .fill(null)
        .map((_, i) => i + 1)
        .map(page => ({ title: page, page }));

      // right
      if (this.current_ < this.totalPage - 3) {
        const start = Math.max(this.current_ + 1, 5);
        const length = this.totalPage - start - 1;
        items.splice(start, length, {
          title: "...",
          page: start + Math.round(length / 2)
        });
      }

      // left
      if (this.current_ - 1 > 3) {
        const start = 1;
        const length = Math.min(this.current_ - 3, this.totalPage - 6);
        items.splice(start, length, {
          title: "...",
          page: start + Math.round(length / 2)
        });
      }

      return items;
    }
  },
  watch: {
    current(value) {
      this.current_ = value;
    }
  },
  methods: {
    changePage(page) {
      if (page !== this.current) {
        this.current_ = page;
        this.$emit("change", page);
        this.$emit("update:current", page);
      }
    },
    prevPage() {
      if (this.current_ > 1) {
        this.changePage(this.current_ - 1);
      }
    },
    nextPage() {
      if (this.current_ < this.totalPage) {
        this.changePage(this.current_ + 1);
      }
    }
  }
};
</script>


<style>
.page-item {
  text-align: center;
  user-select: none;
  display: inline-block;
  margin: 10px;
  width: 30px;
  height: 28px;
  line-height: 30px;
  border-radius: 3px;
  border: 1px solid #ccc;
  cursor: pointer;
}
.active {
  background-color: #37f;
  border: 1px solid #37f;
  color: #fff;
}
</style>
