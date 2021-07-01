<template>
  <div>
    <div class="card-header">
      <div class="offset-4 col-4">
        <h1 class="text-center">Breeding Simulator</h1>
        <div class="row mb-2">
          <label class="col-6">First Lucamon</label>
          <select v-model="firstRarityIndex" class="col-6">
            <option
              v-for="(option, index) in rarities"
              :value="index"
              :key="index"
            >
              {{ option.name }}
            </option>
          </select>
        </div>
        <div class="row mb-2">
          <label class="col-6">Second Lucamon</label>
          <select v-model="secondRarityIndex" class="col-6">
            <option
              v-for="(option, index) in rarities"
              :value="index"
              :key="index"
            >
              {{ option.name }}
            </option>
          </select>
        </div>
        <div class="row mb-2">
          <button class="btn btn-sm btn-warning col-6" @click="reset">
            Reset
          </button>
          <button class="btn btn-sm btn-primary col-6" @click="breed">
            Simulate!
          </button>
        </div>
      </div>
    </div>
    <div class="card-body">
      <div class="offset-3 col-6">
        <h4 class="text-center">Results</h4>
        <div class="table-responsive">
          <table class="table table-bordered w-100" id="data-table">
            <thead>
              <tr>
                <th
                  style="text-align: center; vertical-align: center"
                  rowspan="2"
                >
                  Rarity
                </th>
                <th style="text-align: center" colspan="2">Times</th>
                <th style="text-align: center" colspan="2">Percent</th>
                <th style="text-align: center" colspan="4">
                  First Obtained Round
                </th>
              </tr>
              <tr>
                <th style="text-align: center">Current</th>
                <th style="text-align: center">Total</th>
                <th style="text-align: center">Current</th>
                <th style="text-align: center">Total</th>
                <th style="text-align: center">Current</th>
                <th style="text-align: center">Min</th>
                <th style="text-align: center">Max</th>
                <th style="text-align: center">Average</th>
              </tr>
            </thead>
            <tbody></tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Breeding',
  data() {
    return {
      rarities: [
        {
          name: 'Common',
          weight: 32,
        },
        {
          name: 'Uncommon',
          weight: 16,
        },
        {
          name: 'Rare',
          weight: 8,
        },
        {
          name: 'Epic',
          weight: 4,
        },
        {
          name: 'Ancient',
          weight: 2,
        },
        {
          name: 'Mythical',
          weight: 1,
        },
      ],
      firstRarityIndex: '',
      secondRarityIndex: '',
      percentResult: [],
      weightList: [
        [80, 20, 0, 0, 0, 0],
        [30, 50, 20, 0, 0, 0],
        [20, 30, 30, 20, 0, 0],
        [15, 20, 35, 20, 10, 0],
        [10, 20, 30, 25, 10, 5],
      ],
    };
  },
  methods: {
    breed() {
      let firstTotalWeight = this.rarities[this.firstRarityIndex].weight;
      let secondTotalWeight = this.rarities[this.secondRarityIndex].weight;
      let totalWeight = 0;
      for (let i = 0; i < 6; i++) {
        let firstWeight =
          (firstTotalWeight * this.weightList[this.firstRarityIndex][i]) / 100;
        let secondWeight =
          (secondTotalWeight * this.weightList[this.secondRarityIndex][i]) /
          100;
        this.percentResult[i] = firstWeight + secondWeight;
        totalWeight += this.percentResult[i];
      }
      for (let i = 0; i < 6; i++) {
        this.percentResult[i] = this.getPercent(
          this.percentResult[i],
          totalWeight
        );
      }
    },
    getPercent(number, total) {
      return ((number / total) * 100).toFixed(2);
    },
  },
};
</script>

<style scoped></style>
