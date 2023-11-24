<template>
  <div class="row">
    <p class="csvinputtable-style">CsvInputTableVUE</p>
    <div class="col-md-6">
      <label for="csvFileInput" class="form-label">Load data file</label>
      <input type="file" class="form-control" id="csvFileInput" @change="loadCsvFile" accept=".csv">
    </div>
    
    <div v-if="csvData.length > 0">
      <h3 class="text-center">Input file preview</h3>
      <table class="table">
        <thead>
          <tr>
            <th v-for="(header, index) in csvHeaders" :key="index">{{ header }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(row, rowIndex) in displayedCsvData" :key="rowIndex">
            <td v-for="(value, index) in row" :key="index">{{ value }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import Papa from 'papaparse';


export default {
  name: 'CsvInputTable',
  data() {
    return {
      csvData: [],
      csvHeaders: []
    };
  },
  computed: {
    displayedCsvData() {
      return this.csvData.slice(0, 10);
    }
  },
  methods: {
    loadCsvFile(event) {
      const file = event.target.files[0];
      if (file) {
        this.csvData = [];
        this.csvHeaders = [];
        Papa.parse(file, {
          header: true,
          delimiter: ';',
          step: (row, parser) => {
            this.csvData.push(row.data);
            if (this.csvData.length === 10) {
              parser.abort();
              this.csvHeaders = Object.keys(this.csvData[0]);
            }
          },
          complete: () => {
            // Si el archivo tiene menos de 10 filas, este método se ejecuta después de abortar
            if (!this.csvHeaders.length) {
              this.csvHeaders = Object.keys(this.csvData[0]);
            }
          }
        });
      }
    }
  }
}
</script>

<style>
.csvinputtable-style {
  background-color: rgb(250, 26, 149);
}
</style>