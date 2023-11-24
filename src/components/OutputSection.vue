<template>
    <div>
        <p class="outputsection-style">outputSectionVUE</p>
        <div class="col-md-6">
            <pre>{{ JSON.stringify(filterConfiguration, null, 2) }}</pre>
        </div>
        
    </div>
</template>

<script>


export default {
    props: {
        filterConfiguration: Object
    },
  name: 'OutputSection'
}
</script>

<style>
.outputsection-style {
    background-color: rgb(200, 145, 252);
}
</style>
