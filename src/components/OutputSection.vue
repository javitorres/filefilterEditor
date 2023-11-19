<template>
    <p>Final file:</p>

    
        <div class="row justify-content-center">
            <div class="col-md-6">
                <pre>{{ JSON.stringify(filterConfiguration, null, 2) }}</pre>
            </div>
        </div>
    
</template>

<script>


export default {
  name: 'OutputSection'
}
</script>