<template>
  <div>
    <div>
      <input v-model="filter.keyword" placeholder="Search keyword"/>
      <button @click="downloadCSV()">CSV</button>
    </div>
    <table>
      <thead>.
        <tr>
          <td class="tableHeader" v-for="value in dataColumnList">
            <b>{{ value }}</b>
          </td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="value in valueBody">
          <td v-for="keyvalue in dataColumnList">{{ value[keyvalue] }}</td>
        </tr>
      </tbody>
    </table>
    <div class="pageNavigation">
      <button @click="changePage('previous')">
        <p>Prev</p>
      </button>

      <button v-for="pagenumber in pagination.pageLimit" @click="pagination.page = pagenumber - 1" :class="{ active: (pagenumber === pagination.page + 1) }">
        <p>{{ pagenumber }}</p>
      </button>

      <button @click="changePage('next')">
        <p>Next</p>
      </button>
    </div>
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

  button.active {
    border: 3px solid orange
  }

</style>

<script>

export default {
  name: 'vue-simple-table',
  props: ['inputdata', 'configdata'],

  data () {
    return {
      dataColumnList: [],

      filter: {
        keyword: null,
      },

      pagination: {
        page: 0,
        pageLimit: 0,
        size: 30
      },

      mode: 'data',

      config: {
        filename: ''
      },
    }
  },

  created () {
    this.dataColumnList = Object.keys(this.inputdata[0])
    this.initConfig(this.configdata)
  },

  computed: {
    valueBody (mode) {
      const reg = new RegExp(this.filter.keyword, 'ig')
      const filtered = this.inputdata.filter(entry => {
        if (!this.filter.keyword) {
          return entry
        }

        for (let i = 0; i < this.dataColumnList.length; i++) {
          if (reg.test(entry[this.dataColumnList[i]])) {
            return entry
          }
        }
      })

      if (this.mode === 'csv') {
        return filtered
      }

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
    },

    downloadCSV (d) {
      const filename = `${this.config.filename}_${new Date().toLocaleString()}.csv`
      const csvRow = [this.dataColumnList]

      let csvFile = ''

      this.mode = 'csv'

      this.valueBody.map(data => {
        const dataRow = []
        Object.values(data).map(value => { dataRow.push(`${value}`) })
        csvRow.push(dataRow)
      })

      for (let i = 0; i < csvRow.length; i++) {
        csvFile += this.processRow(csvRow[i])
      }

      const blob = new window.Blob([csvFile], { type: 'text/csv;charset=utf-8,%EF%BB%BF;' })

      if (navigator.msSaveBlob) {
        navigator.msSaveBlob(blob, filename)
      } else {
        const link = document.createElement('a')
        const url = URL.createObjectURL(blob)

        if (link.download !== undefined) {
          link.setAttribute('href', url)
          link.setAttribute('download', filename)
          link.style.visibility = 'hidden'

          document.body.appendChild(link)
          link.click()
          document.body.removeChild(link)
        }
      }

      this.mode = ''
    },

    initConfig (configdata) {
      this.config.filename = configdata.filename || 'data'
    },

    processRow (row) {
      let finalVal = ''

      for (let j = 0; j < row.length; j++) {
        const innerValue = row[j] === null ? '' : row[j].toString()
        const result = '"' + innerValue.replace(/"/g, '""') + '"'

        if (j > 0) {
          finalVal += ','
        }

        finalVal += result
      }

      finalVal = finalVal.replace(/，/g, ',')

      return finalVal + '\n'
    }
  }
}
</script>
