<template>
  <div class="card shadow">
    <div class="card-header">
      <div class="offset-4 col-4">
        <h1 class="text-center">Lucamon Random Simulator</h1>
        <div class="row mb-2">
          <label class="col-6" for="commonPercent">Common</label>
          <input
            v-model="commonPercent"
            type="number"
            id="commonPercent"
            class="offset-2 col-4 text-end"
          />
        </div>
        <div class="row mb-2">
          <label class="col-6" for="uncommonPercent">Uncommon</label>
          <input
            v-model="uncommonPercent"
            type="number"
            id="uncommonPercent"
            class="offset-2 col-4 text-end"
          />
        </div>
        <div class="row mb-2">
          <label class="col-6" for="rarePercent">Rare</label>
          <input
            v-model="rarePercent"
            type="number"
            id="rarePercent"
            class="offset-2 col-4 text-end"
          />
        </div>
        <div class="row">
          <label class="col-6" for="epicPercent">Epic</label>
          <input
            v-model="epicPercent"
            type="number"
            id="epicPercent"
            class="offset-2 col-4 text-end"
          />
        </div>
        <div class="row mb-4">
          <span class="offset-6 col-6 text-end">Total {{ totalPercent }}%</span>
        </div>
        <div class="row mb-2">
          <label class="col-6" for="times">Times</label>
          <input
            v-model="times"
            type="number"
            id="times"
            class="offset-2 col-4 text-end"
          />
        </div>
        <div class="row mb-2">
          <button class="btn btn-sm btn-warning col-6" @click="reset">
            Reset
          </button>
          <button
            class="btn btn-sm btn-primary col-6"
            @click="simulate"
            :disabled="totalPercent !== 100 || times === '' || times <= 0"
          >
            Simulate!
          </button>
        </div>
      </div>
    </div>
    <div class="card-body">
      <div class="offset-3 col-6">
        <h4 class="text-center">
          Results {{ resultTimes }} times (From
          {{ simulationCount }} simulations with total {{ totalTimes }} times)
        </h4>
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
            <tbody>
              <template v-for="(rarity, index) in rarityList" :key="index">
                <tr>
                  <td>{{ rarity.head }}</td>
                  <td style="text-align: right">
                    {{ rarity.times.current }}
                  </td>
                  <td style="text-align: right">
                    {{ rarity.times.total }}
                  </td>
                  <td style="text-align: right">
                    {{ rarity.actualPercent.current }}%
                  </td>
                  <td style="text-align: right">
                    {{ rarity.actualPercent.total }}%
                  </td>
                  <td style="text-align: right">
                    {{ rarity.firstObtainedRound.current }}
                  </td>
                  <td style="text-align: right">
                    {{ rarity.firstObtainedRound.min }}
                  </td>
                  <td style="text-align: right">
                    {{ rarity.firstObtainedRound.max }}
                  </td>
                  <td style="text-align: right">
                    {{ rarity.firstObtainedRound.average }}
                  </td>
                </tr>
              </template>
            </tbody>
          </table>
        </div>
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
  name: 'App',
  data() {
    return {
      rarityIndex: {
        common: 0,
        uncommon: 1,
        rare: 2,
        epic: 3,
      },
      times: '',
      simulationCount: 0,
      resultTimes: 0,
      totalTimes: 0,
      totalPercent: 100,
      commonPercent: 50,
      uncommonPercent: 40,
      rarePercent: 9,
      epicPercent: 1,
      rarityList: [
        {
          head: 'Common',
          times: {
            current: 0,
            total: 0,
          },
          actualPercent: {
            current: 0,
            total: 0,
          },
          firstObtainedRound: {
            current: 0,
            min: 0,
            max: 0,
            average: 0,
            total: 0,
          },
        },
        {
          head: 'Uncommon',
          times: {
            current: 0,
            total: 0,
          },
          actualPercent: {
            current: 0,
            total: 0,
          },
          firstObtainedRound: {
            current: 0,
            min: 0,
            max: 0,
            average: 0,
            total: 0,
          },
        },
        {
          head: 'Rare',
          times: {
            current: 0,
            total: 0,
          },
          actualPercent: {
            current: 0,
            total: 0,
          },
          firstObtainedRound: {
            current: 0,
            min: 0,
            max: 0,
            average: 0,
            total: 0,
          },
        },
        {
          head: 'Epic',
          times: {
            current: 0,
            total: 0,
          },
          actualPercent: {
            current: 0,
            total: 0,
          },
          firstObtainedRound: {
            current: 0,
            min: 0,
            max: 0,
            average: 0,
            total: 0,
          },
        },
      ],
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
  computed: {
    commonPercentNum: function () {
      return parseFloat(this.commonPercent);
    },
    uncommonPercentNum: function () {
      return parseFloat(this.uncommonPercent);
    },
    rarePercentNum: function () {
      return parseFloat(this.rarePercent);
    },
    epicPercentNum: function () {
      return parseFloat(this.epicPercent);
    },
  },
  watch: {
    commonPercent() {
      this.calculateTotalPercent();
    },
    uncommonPercent() {
      this.calculateTotalPercent();
    },
    rarePercent() {
      this.calculateTotalPercent();
    },
    epicPercent() {
      this.calculateTotalPercent();
    },
  },
  methods: {
    reset() {
      this.times = '';
      this.simulationCount = 0;
      this.resultTimes = 0;
      this.totalTimes = 0;
      this.commonPercent = 50;
      this.uncommonPercent = 40;
      this.rarePercent = 9;
      this.epicPercent = 1;
      this.rarityList = [
        {
          head: 'Common',
          times: {
            current: 0,
            total: 0,
          },
          actualPercent: {
            current: 0,
            total: 0,
          },
          firstObtainedRound: {
            current: 0,
            min: 0,
            max: 0,
            average: 0,
            total: 0,
          },
        },
        {
          head: 'Uncommon',
          times: {
            current: 0,
            total: 0,
          },
          actualPercent: {
            current: 0,
            total: 0,
          },
          firstObtainedRound: {
            current: 0,
            min: 0,
            max: 0,
            average: 0,
            total: 0,
          },
        },
        {
          head: 'Rare',
          times: {
            current: 0,
            total: 0,
          },
          actualPercent: {
            current: 0,
            total: 0,
          },
          firstObtainedRound: {
            current: 0,
            min: 0,
            max: 0,
            average: 0,
            total: 0,
          },
        },
        {
          head: 'Epic',
          times: {
            current: 0,
            total: 0,
          },
          actualPercent: {
            current: 0,
            total: 0,
          },
          firstObtainedRound: {
            current: 0,
            min: 0,
            max: 0,
            average: 0,
            total: 0,
          },
        },
      ];
    },
    simulate() {
      if (this.totalPercent !== 100) {
        return alert('ใส่ให้ครบ 100% ไอเหี้ย');
      }
      if (this.times === '' || this.times <= 0) {
        return alert('ใส่จำนวนครั้งให้ถูกด้วยไอเวร');
      }

      this.random();
    },
    calculateTotalPercent() {
      let commonPercent = 0;
      let uncommonPercent = 0;
      let rarePercent = 0;
      let epicPercent = 0;
      if (this.isNumber(this.commonPercent)) {
        commonPercent = this.commonPercentNum;
      }
      if (this.isNumber(this.uncommonPercent)) {
        uncommonPercent = this.uncommonPercentNum;
      }
      if (this.isNumber(this.rarePercent)) {
        rarePercent = this.rarePercentNum;
      }
      if (this.isNumber(this.epicPercent)) {
        epicPercent = this.epicPercentNum;
      }
      this.totalPercent =
        commonPercent + uncommonPercent + rarePercent + epicPercent;
    },
    isNumber(val) {
      return val !== '' && !isNaN(val);
    },
    random() {
      let common = this.rarityList[this.rarityIndex.common];
      let uncommon = this.rarityList[this.rarityIndex.uncommon];
      let rare = this.rarityList[this.rarityIndex.rare];
      let epic = this.rarityList[this.rarityIndex.epic];
      common.firstObtainedRound.current = 0;
      common.times.current = 0;
      uncommon.firstObtainedRound.current = 0;
      uncommon.times.current = 0;
      rare.firstObtainedRound.current = 0;
      rare.times.current = 0;
      epic.firstObtainedRound.current = 0;
      epic.times.current = 0;
      for (let round = 1; round <= this.times; round++) {
        let rarityIndex = this.getRandomRarityIndex();
        let rarityModel = this.rarityList[rarityIndex];
        if (rarityModel.firstObtainedRound.current === 0) {
          rarityModel.firstObtainedRound.current = round;
          rarityModel.firstObtainedRound.total += round;
          if (rarityModel.firstObtainedRound.min === 0) {
            rarityModel.firstObtainedRound.min = round;
            rarityModel.firstObtainedRound.max = round;
          } else if (round < rarityModel.firstObtainedRound.min) {
            rarityModel.firstObtainedRound.min = round;
          } else if (round > rarityModel.firstObtainedRound.max) {
            rarityModel.firstObtainedRound.max = round;
          }
        }
        rarityModel.times.current++;
      }
      this.resultTimes = parseInt(this.times);
      this.totalTimes += this.resultTimes;
      this.simulationCount++;
      for (let index = 0; index < this.rarityList.length; index++) {
        let rarityModel = this.rarityList[index];
        rarityModel.actualPercent.current = this.getPercent(
          rarityModel.times.current,
          this.times
        );
        rarityModel.times.total += rarityModel.times.current;
        rarityModel.actualPercent.total = this.getPercent(
          rarityModel.times.total,
          this.totalTimes
        );
        rarityModel.firstObtainedRound.average = (
          rarityModel.firstObtainedRound.total / this.simulationCount
        ).toFixed(2);
      }
    },
    getRandomInt(min, max) {
      return Math.floor(Math.random() * max) + min;
    },
    getPercent(number, total) {
      return ((number / total) * 100).toFixed(2);
    },
    getRandomRarityIndex() {
      let randomizedNumber = this.getRandomInt(1, 10000);
      let commonZone = this.commonPercentNum * 100;
      let uncommonZone = this.uncommonPercentNum * 100 + commonZone;
      let rareZone = this.rarePercentNum * 100 + uncommonZone;
      if (randomizedNumber <= commonZone) {
        return this.rarityIndex.common;
      } else if (randomizedNumber <= uncommonZone) {
        return this.rarityIndex.uncommon;
      } else if (randomizedNumber <= rareZone) {
        return this.rarityIndex.rare;
      } else {
        return this.rarityIndex.epic;
      }
    },
    randomHashRate(index) {
      let growthModel = this.growthModels[index];
      let hashRate = this.getRandomInt(
        growthModel.minHashRate,
        growthModel.maxHashRate - growthModel.minHashRate + 1
      );
      growthModel.randomHashRate = hashRate;
    },
    growth(index) {
      let growthModel = this.growthModels[index];
      let growth = this.getRandomInt(
        growthModel.growthMin,
        growthModel.growthMax - growthModel.growthMin + 1
      );
      growthModel.randomHashRate += growth;
    },
  },
};
</script>

<style>
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>
