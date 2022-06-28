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
    <v-row v-for="(result, keyword) in results" :key="keyword">
      <v-col cols="4">
        <div class="img-container mx-2">
          <h1 class="text-center">{{keyword}}</h1>
        </div>
      </v-col>
      <v-col cols="8">
        <v-sheet
            class="pa-2"
            elevation="8"
            max-width="1100"
            style="overflow: hidden"
        >
          <p class="d-none"></p>
          <div v-if="Object.keys(result).length">
            <div v-for="(value, key) in result" :key="key" class="ml-4 my-2">
              <h3 class="d-inline">{{ key }}: </h3>
              <span>{{ value }}</span>
            </div>
          </div>
          <h3 v-else class="py-2 px-2">Result not found...</h3>
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