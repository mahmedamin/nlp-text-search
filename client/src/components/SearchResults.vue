<template>
  <v-row
      v-if="isLoading"
      class="fill-height"
      align-content="center"
      justify="center"
  >
    <v-col class="text-subtitle-1 text-center" cols="12">
      Getting your results...
    </v-col>
    <v-col cols="4">
      <v-progress-linear indeterminate rounded height="6"></v-progress-linear>
    </v-col>
  </v-row>
  <div v-else class="mt-4">
    <v-row v-for="(result, keyword) in results">
      <v-col cols="4">
        <div class="img-container mx-2">
          <h1 class="text-center text-decoration-underline">{{keyword}}</h1>
        </div>
      </v-col>
      <v-col cols="8">
        <v-sheet
            class="pa-2"
            elevation="8"
            max-width="1100"
            style="overflow: hidden"
        >
          <div v-for="(value, key) in result" :key="attribute" class="ml-4 my-2">
            <h3 class="d-inline">{{ key.replace(/([A-Z])/g, ' $1')
                .replace(/^./, str => str.toUpperCase()) }}: </h3>
              <span>{{ value }}</span>
          </div>
        </v-sheet>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  name: "SearchResults",
  props: {
    isLoading: Boolean,
    results: Object
  },
  data() {
    return {}
  },
}
</script>

<style scoped>

</style>