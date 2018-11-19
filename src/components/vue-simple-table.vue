<template>
  <div>
    <div>
      <input v-model="filter.keyword" />
    </div>
    <table>
      <thead>
        <tr>
          <td class="tableHeader" v-for="value in valueHeader">
            <b>{{ value }}</b>
          </td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="value in valueBody">
          <td v-for="keyvalue in valueHeader">{{ value[keyvalue] }}</td>
        </tr>
      </tbody>
    </table>
    <div class="pageNavigation">
      <button @click="changePage('previous')">
        <p>Prev</p>
      </button>
      <button v-for="pagenumber in pagination.pageLimit" @click="pagination.page = pagenumber - 1">
        <p>{{ pagenumber }}</p>
      </button>
      <button @click="changePage('next')">
        <p>Next</p>
      </button>
    </div>

    <!-- .field.is-horizontal.nav-pagination
      .control.buttons.has-addons
        .button(@click="changePage('previous', 'configWinners')")
          i.fas.fa-angle-left
        .button(v-for="page in configWinners.pageLimit" v-bind:class="{ active: (page === configWinners.page + 1) }" @click="changePage(page, 'configWinners')")
          p {{ page }}
        .button(@click="changePage('next', 'configWinners')")
          i.fas.fa-angle-right -->
  </div>
</template>

<style scoped>
  table {
    border-collapse: collapse;
    width: 100%;
  }

  th, td {
    text-align: left;
    padding: 8px;
  }

  tr:nth-child(even) {
    background-color: #f2f2f2
  }

  .tableHeader {
    text-transform: capitalize
  }

</style>

<script>

export default {
  name: 'vue-simple-table',
  props: ['inputdata'],
  data () {
    return {
      valueHeader: [],

      filter: {
        keyword: null,
      },

      pagination: {
        page: 0,
        pageLimit: 0,
        size: 30
      }
    }
  },
  created () {
    this.valueHeader = Object.keys(this.inputdata[0])
  },

  computed: {
    valueBody () {
      const reg = new RegExp(this.filter.keyword, 'ig')
      const filtered = this.inputdata.filter(entry => {
        if (!this.filter.keyword) {
          return entry
        }

        for (let i = 0; i < this.valueHeader.length; i++) {
          if (reg.test(entry[this.valueHeader[i]])) {
            return entry
          }
        }
      })

      this.pagination.pageLimit = Math.ceil(filtered.length / this.pagination.size)
      const slicePagination = filtered.slice(this.pagination.page * this.pagination.size, (this.pagination.page + 1) * this.pagination.size)

      return slicePagination
    }
  },

  methods: {
    changePage (d) {
      if (d === 'previous') {
        return this.pagination.page = Math.max(0, this.pagination.page - 1)
      }

      if (d === 'next') {
        return this.pagination.page = Math.min(this.pagination.pageLimit - 1, this.pagination.page + 1)
      }
    }
  }
}
</script>
