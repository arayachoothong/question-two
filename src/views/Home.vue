<template>
  <div class="container">
    <div class="input-wrap">
      <input
        type="text"
        id="search"
        name="search"
        class="input-search"
        v-model="search"
        placeholder="Search"
        @keyup="onFilterData"
      />
    </div>
    <div>
      <table>
        <tr>
          <th>Category</th>
        </tr>
        <template v-if="dataFilterList.length > 0">
          <tr
            v-for="(item, index) in dataFilterList"
            :key="'category-' + index"
          >
            <td>{{ item }}</td>
          </tr>
        </template>
        <template v-else>
          <tr class="">
            <td class="no-data">ไม่พบข้อมูล</td>
          </tr>
        </template>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dataList: [],
      dataFilterList: [],
      search: "",
    };
  },
  created: async function () {
    await this.getCategories();
  },
  methods: {
    onFilterData() {
      if (this.search) {
        this.dataFilterList = this.dataList.filter((el) =>
          el.includes(this.search)
        );
      } else {
        this.dataFilterList = this.dataList;
      }
    },
    getCategories: async function () {
      await this.axios
        .get("https://api.publicapis.org/categories")
        .then((response) => {
          this.dataList = response.data;
          this.dataFilterList = response.data;
        });
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  margin: 5rem;
}

.input-search {
  width: 100%;
  padding-top: 0.5rem;
  padding-bottom: 0.5rem;
}

.input-wrap {
  margin-top: 1rem;
  margin-bottom: 1rem;
  margin-right: 0.5rem;
}

table {
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}
.no-data {
  text-align: center !important;
}
</style>