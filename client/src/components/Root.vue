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
          <v-col cols="4" class="mt-5">
            <v-radio-group class="float-right" row v-model="radioGrp">
              <v-radio
                  v-for="btn in radioButtons"
                  :key="btn.value"
                  :label="btn.label"
                  :value="btn.value"
              ></v-radio>
            </v-radio-group>
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
              v-if="radioGrp === 'searchKeyword'"
              v-model="searchKeyword"
              required
              placeholder="Enter search keyword"
              prepend-icon="mdi-magnify"
              label="Search keyword"
              clearable
            ></v-text-field>
            <v-file-input
                v-if="radioGrp === 'fileUpload'"
                v-model="xlsxFile"
                required
                :multiple="false"
                accept="text/csv,application/vnd.openxmlformats-officedocument.spreadsheetml.sheet,	application/csv"
                placeholder="Select an Excel File"
                prepend-icon="mdi-file-excel-box"
                label="Excel File"
            ></v-file-input>
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
  parseExcel
} from "../utils/utils";

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
      xlsxFile: null,
      radioButtons: [
        { value: "searchKeyword", label: "Text Search" },
        { value: "fileUpload", label: "File Upload" },
      ],
      radioGrp: "searchKeyword",
      resultsData: {},
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
      this.resultsData = {};
      this.isLoading = true;
      let keywords = [];

      switch (this.radioGrp) {
        case 'searchKeyword':
          if (!this.searchKeyword)
            return false;

          keywords.push(this.searchKeyword);
          break;
        case 'fileUpload':
          let excelData = JSON.parse(await parseExcel(this.xlsxFile));
          if (!excelData.length)
            return false;

          let firstKeyword;
          excelData.forEach((object, key) => {
            if (0 === key) {
              firstKeyword = Object.keys(object)[0];
              keywords.push(firstKeyword);
            }

            keywords.push(object[firstKeyword]);
          });
          break;
      }

      for (const keyword of keywords) {
        const results = await WSIMLSearchService.getNlpResults(this.searchKeyword);
        if (results.error) {
          this.isLoading = false;
          this.isError = true;
          this.errorDetail = results.message;
          return;
        }

        this.resultsData[keyword] = results;
      }

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
