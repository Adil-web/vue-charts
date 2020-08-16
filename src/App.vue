<template>
  <div class="container-fluid" style="width: 600px;">
        <div class="row">
            <div class="col-4" v-if="arrRegions.length > 0">
                <RegionMortality
                :chartData="arrRegions"
                :chartColors="regionsChartColors"
                label="mortality"
                />
                <MortalityYears
                :chartData="arrYears"
                :chartColors="regionsChartColors"
                label="mortality"
                />
            </div>
            <div class="col-4" v-if="arrYears.length > 0">
                <FertilityRegion
                :chartData="arrRegions"
                :chartColors="regionsChartColors"
                label="fertility"
                />
                <FertilityYear
                :chartData="arrYears"
                :chartColors="regionsChartColors"
                label="fertility"
                />
            </div>
            <div class="col-4" v-if="arrYears.length > 0">
                <PopulationRegion
                :chartData="arrRegions"
                :chartColors="regionsChartColors"
                label="populations"
                />
                <PopulationYears
                :chartData="arrYears"
                :chartColors="regionsChartColors"
                label="population"
                />
            </div>
        </div>
        <div class="row" v-if="arrYears.length > 0" >
            <div class="col-12">
                <ExpectancyYears
                :height="100"
                :chartData="arrYears"
                :chartColors="regionsChartColors"
                label="expectancy"
                />
            </div>
        </div>
  </div>
</template>

<script>
import axios from "axios";

import RegionMortality from "./components/RegionMortality";
import MortalityYears from "./components/MortalityYears";

import FertilityRegion from "./components/FertilityRegion";
import FertilityYear from "./components/FertilityYears";

import PopulationRegion from "./components/PopulationRegion";
import PopulationYears from "./components/PopulationYears";

import ExpectancyYears from "./components/ExpectancyYears";

export default {
  components: {
    RegionMortality,
    MortalityYears,
    FertilityRegion,
    FertilityYear,
    PopulationRegion,
    PopulationYears,
    ExpectancyYears
  },
  data() {
    return {
      arrRegions: [],
      regionsChartColors: {
        borderColor: "#077187",
        pointBorderColor: "#0E1428",
        pointBackgroundColor: "#AFD6AC",
        backgroundColor: "#74A57F"
      },
      arrYears: []
    };
  },
  created() {
    axios.all([
        axios.get('http://localhost:8090/api/demography/regions'),
        axios.get('http://localhost:8090/api/demography/years')
      ])
    .then(axios.spread((first_response, second_response) => {
        this.arrRegions = first_response.data
        this.arrYears = second_response.data
    }))
  },
};
</script>
