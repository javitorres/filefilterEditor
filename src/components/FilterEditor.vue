<template>
    <p>FilterEditor for actionType: {{ actionType }}</p>
    <textarea class="form-control" rows="1" v-model="name"></textarea>
    <!-- Toggle Disabled -->
    <input type="checkbox" v-model="disabled" id="disabled" title="rrr">
    <textarea class="form-control" rows="10" v-model="code"></textarea>

    <!-- Delete filter button -->
    <button class="btn btn-danger" @click="deleteFilter">Delete filter</button>
    <!-- Test filter -->
    <button class="btn btn-primary" @click="testFilter">Test filter</button>

</template>
<script>
  export default {
    name: 'FilterEditor',
    props: {
      actionType: String
    },
    data() {
      return {
        name: 'Filter Name',
        disabled: false,
        code: 'Write here your code'
      }
    },
    methods: {
      deleteFilter() {
        this.$emit('deleteFilter');
      },
      testFilter() {
        this.$emit('testFilter');
      },
      emitChanges() {
      this.$emit('filterChanged',{ 
        name: this.name,
        disabled: this.disabled,
        code: this.code,
      }
    );
    }
    },
    watch: {
      name() {
        this.emitChanges();
      },
      code() {
        this.emitChanges();
      },
      disabled() {
        this.emitChanges();
      }
    }
  };
</script>