<template>
  <div>
    <div class="card-header"></div>
    <div class="card-body">
      <div class="offset-3 col-6">
        <h4 class="text-center">Results</h4>
        <div class="table-responsive">
          <h4 class="text-center">Growth Simulator</h4>
          <table class="table table-bordered w-100" id="data-table">
            <thead>
              <tr>
                <th
                  style="text-align: center; vertical-align: center"
                  rowspan="2"
                >
                  Rarity
                </th>
                <th style="text-align: center" colspan="2">Hash Rate</th>
                <th style="text-align: center" colspan="2">Growth Range</th>
                <th style="text-align: center" rowspan="2">Random Hash Rate</th>
                <th style="text-align: center" rowspan="2">Actions</th>
              </tr>
              <tr>
                <th style="text-align: center">Min</th>
                <th style="text-align: center">Max</th>
                <th style="text-align: center">Min</th>
                <th style="text-align: center">Max</th>
              </tr>
            </thead>
            <tbody>
              <template
                v-for="(growthModel, index) in growthModels"
                :key="index"
              >
                <tr>
                  <td>{{ growthModel.head }}</td>
                  <td style="text-align: right">
                    {{ growthModel.minHashRate }}
                  </td>
                  <td style="text-align: right">
                    {{ growthModel.maxHashRate }}
                  </td>
                  <td style="text-align: right">
                    {{ growthModel.growthMin }}
                  </td>
                  <td style="text-align: right">
                    {{ growthModel.growthMax }}
                  </td>
                  <td style="text-align: right">
                    {{ growthModel.randomHashRate }}
                  </td>
                  <td class="text-center">
                    <button
                      class="btn btn-sm btn-primary mr-2"
                      @click="randomHashRate(index)"
                    >
                      Random Hash Rate
                    </button>
                    <button class="btn btn-sm btn-info" @click="growth(index)">
                      Growth
                    </button>
                  </td>
                </tr>
              </template>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Growth',
  data() {
    return {
      growthModels: [
        {
          head: 'Common',
          minHashRate: 1,
          maxHashRate: 3,
          growthMin: 1,
          growthMax: 2,
          randomHashRate: 0,
        },
        {
          head: 'Uncommon',
          minHashRate: 2,
          maxHashRate: 5,
          growthMin: 2,
          growthMax: 4,
          randomHashRate: 0,
        },
        {
          head: 'Rare',
          minHashRate: 6,
          maxHashRate: 10,
          growthMin: 6,
          growthMax: 8,
          randomHashRate: 0,
        },
        {
          head: 'Epic',
          minHashRate: 15,
          maxHashRate: 50,
          growthMin: 15,
          growthMax: 40,
          randomHashRate: 0,
        },
        {
          head: 'Ancient',
          minHashRate: 100,
          maxHashRate: 150,
          growthMin: 80,
          growthMax: 120,
          randomHashRate: 0,
        },
        {
          head: 'Mythical',
          minHashRate: 120,
          maxHashRate: 180,
          growthMin: 120,
          growthMax: 180,
          randomHashRate: 0,
        },
      ],
    };
  },
  methods: {
    randomHashRate(index) {
      let growthModel = this.growthModels[index];
      growthModel.randomHashRate = this.getRandomInt(
        growthModel.minHashRate,
        growthModel.maxHashRate - growthModel.minHashRate + 1
      );
    },
    growth(index) {
      let growthModel = this.growthModels[index];
      let growth = this.getRandomInt(
        growthModel.growthMin,
        growthModel.growthMax - growthModel.growthMin + 1
      );
      growthModel.randomHashRate += growth;
    },
    getRandomInt(min, max) {
      return Math.floor(Math.random() * max) + min;
    },
  },
};
</script>

<style scoped></style>
