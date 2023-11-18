<template>
  <div>
    

    <div v-for="(filter, index) in filters" :key="index" class="mb-3">
      <filter-editor :type="filter.type" @updateFilter="updateFilter(index, $event)"></filter-editor>
    </div>

    <div class="container text-center" wid>
      <div class="row">
        <div class="col">
          <button class="btn btn-primary" @click="addFilter('python')">Añadir Filtro Python</button>
        </div>
        <div class="col">
          <button class="btn btn-primary" @click="addFilter('pandas')">Añadir Filtro Pandas</button>
        </div>
        <div class="col">
          <button class="btn btn-primary" @click="addFilter('sql')">Añadir Filtro SQL</button>
        </div>
      </div>
  </div>
  </div>
</template>


<script>
  import FilterEditor from './FilterEditor.vue';

  export default {
    name: 'FilterConfigurator',
    components: {
      FilterEditor
    },
    data() {
      return {
        filters: []
      };
    },
    methods: {
      addFilter(type) {
        this.filters.push({ type, config: {} });
      },
      updateFilter(index, config) {
        this.filters[index].config = config;
      }
    }
  };
</script>
