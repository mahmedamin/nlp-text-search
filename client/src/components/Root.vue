<template>
  <v-app id="inspire">
    <v-app-bar app>
      <v-form
        ref="form"
        style="text-align: center; width: 100%"
        v-on:submit.prevent
      >
        <v-row align="center" justify="center" no-gutters>
          <v-col cols="3">
            <img
              class="float-left mt-1"
              style="height: 40px"
              src="https://assets.wsimgs.com/wsimgs/rk/images/i/202143/0006/images/common/logo.svg"
            />
          </v-col>
          <v-col cols="5" class="mt-5">
            <v-btn
              @click="onClickSearch()"
              type="submit"
              dark
              class="float-right ml-5 mb-3"
              elevation="2"
            >
              SEARCH
            </v-btn>

            <v-text-field
              v-model="searchKeyword"
              required
              placeholder="Enter search keyword"
              prepend-icon="mdi-magnify"
              label="Search keyword"
              clearable
            ></v-text-field>
          </v-col>
        </v-row>
      </v-form>
    </v-app-bar>
    <v-snackbar v-model="isError">
      {{ errorDetail }}
      <template v-slot:action="{ attrs }">
        <v-btn color="pink" text v-bind="attrs" @click="onSnackBarClose">
          Close
        </v-btn>
      </template>
    </v-snackbar>
    <v-main>
      <search-results :is-loading="isLoading" :results="resultsData"></search-results>
    </v-main>
  </v-app>
</template>

<script>
import WSIMLSearchService from "../services/WSIMLSearch.service";
import {
} from "../utils/utils";
import { BRANDS, TAB_STATUSES } from "../constants/constants";

import { mapMutations, mapGetters } from "vuex";
import SearchResults from "./SearchResults";

export default {
  components: {
    SearchResults
  },

  mounted() {
  },

  data() {
    return {
      searchKeyword: null,
      resultsData: {},
      filters: {
        priceRange: {
          min: null,
          max: null,
        },
      },
      isLoading: false,
      isError: false,
      errorDetail: "",
    };
  },
  watch: {
  },
  computed: {
    ...mapGetters([
    ]),
  },
  methods: {
    ...mapMutations([]),
    onSnackBarClose() {
      this.isError = false;
      this.errorDetail = "";
    },
    async onClickSearch() {
      if (!this.searchKeyword)
        return false;

      this.isLoading = true;

      const results = await WSIMLSearchService.getNlpResults(this.searchKeyword);
      this.resultsData[this.searchKeyword] = results;

      this.isLoading = false;
    },
  },
};
</script>
<style >
#inspire
  > div
  > main
  > div
  > div.v-window.v-item-group.theme--light.v-tabs-items
  > div
  > div
  > div
  > div.cropper-container.cropper-bg {
  display: none !important;
}
</style>
