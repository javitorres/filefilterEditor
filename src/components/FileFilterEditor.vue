<template>
  <div class="container my-4">
    <h1 class="text-center text-muted">Editor de Filtros de Archivos</h1>

    <!-- Sección de Carga y Visualización del CSV de Entrada -->
    <csv-input-table></csv-input-table>

    <!-- Sección de Configuración de Filtros 
    <filter-configurator></filter-configurator>
    -->

    <!-- Sección de Vista Previa y Descarga 
    <output-section></output-section>
    -->
  </div>
</template>


<script>
import CsvInputTable from './CsvInputTable.vue';
//import FilterConfigurator from './FilterConfigurator.vue';
//import OutputSection from './OutputSection.vue';

export default {
  name: 'FileFilterEditor',
  components: {
    CsvInputTable
  },
  props: {
    msg: String
  }
}
</script>


