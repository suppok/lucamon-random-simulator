<template>
  <div>
    <div class="card-header">
      <div class="offset-4 col-4">
        <h1 class="text-center">Breeding Simulator</h1>
        <div class="row mb-2">
          <label class="col-6">First Lucamon</label>
          <select v-model="firstRarityIndex" class="offset-2 col-4 text-end">
            <option value="" disabled selected>Please Select</option>
            <option
              v-for="(option, index) in rarities.slice(0, rarities.length - 1)"
              :value="index"
              :key="index"
            >
              {{ option.name }}
            </option>
          </select>
        </div>
        <div class="row mb-2">
          <label class="col-6">Second Lucamon</label>
          <select v-model="secondRarityIndex" class="offset-2 col-4 text-end">
            <option value="" disabled selected>Please Select</option>
            <option
              v-for="(option, index) in rarities.slice(0, rarities.length - 1)"
              :value="index"
              :key="index"
            >
              {{ option.name }}
            </option>
          </select>
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
            @click="breed"
            :disabled="
              firstRarityIndex === '' ||
              secondRarityIndex === '' ||
              times === '' ||
              times <= 0
            "
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
                <th style="text-align: center" colspan="3">Percent</th>
                <th style="text-align: center" colspan="4">
                  First Obtained Round
                </th>
              </tr>
              <tr>
                <th style="text-align: center">Current</th>
                <th style="text-align: center">Total</th>
                <th style="text-align: center">Actual</th>
                <th style="text-align: center">Current</th>
                <th style="text-align: center">Total</th>
                <th style="text-align: center">Current</th>
                <th style="text-align: center">Min</th>
                <th style="text-align: center">Max</th>
                <th style="text-align: center">Average</th>
              </tr>
            </thead>
            <tbody>
              <template v-for="(rarity, index) in rarities" :key="index">
                <tr>
                  <td>{{ rarity.name }}</td>
                  <td style="text-align: right">
                    {{ rarity.times.current }}
                  </td>
                  <td style="text-align: right">
                    {{ rarity.times.total }}
                  </td>
                  <td style="text-align: right">
                    <b>{{ percentResult[index] }}%</b>
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
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Breeding',
  data() {
    return {
      times: '',
      simulationCount: 0,
      resultTimes: 0,
      totalTimes: 0,
      rarityIndex: {
        common: 0,
        uncommon: 1,
        rare: 2,
        epic: 3,
        ancient: 4,
        mythical: 5,
      },
      rarities: [
        {
          name: 'Common',
          weight: 32,
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
          name: 'Uncommon',
          weight: 16,
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
          name: 'Rare',
          weight: 8,
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
          name: 'Epic',
          weight: 4,
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
          name: 'Ancient',
          weight: 2,
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
          name: 'Mythical',
          weight: 1,
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
      firstRarityIndex: '',
      secondRarityIndex: '',
      percentResult: [0, 0, 0, 0, 0, 0],
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
    reset() {
      this.times = '';
      this.simulationCount = 0;
      this.resultTimes = 0;
      this.totalTimes = 0;
      this.rarities = [
        {
          name: 'Common',
          weight: 32,
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
          name: 'Uncommon',
          weight: 16,
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
          name: 'Rare',
          weight: 8,
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
          name: 'Epic',
          weight: 4,
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
          name: 'Ancient',
          weight: 2,
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
          name: 'Mythical',
          weight: 1,
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
      this.firstRarityIndex = '';
      this.secondRarityIndex = '';
      this.percentResult = [0, 0, 0, 0, 0, 0];
    },
    breed() {
      if (this.firstRarityIndex === '' || this.secondRarityIndex === '') {
        return alert('เลือก rarity ดิวะ');
      }

      this.calculatePercent();
      this.randomRarity();
    },
    calculatePercent() {
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
    randomRarity() {
      let common = this.rarities[this.rarityIndex.common];
      let uncommon = this.rarities[this.rarityIndex.uncommon];
      let rare = this.rarities[this.rarityIndex.rare];
      let epic = this.rarities[this.rarityIndex.epic];
      let ancient = this.rarities[this.rarityIndex.ancient];
      let mythical = this.rarities[this.rarityIndex.mythical];
      common.firstObtainedRound.current = 0;
      common.times.current = 0;
      uncommon.firstObtainedRound.current = 0;
      uncommon.times.current = 0;
      rare.firstObtainedRound.current = 0;
      rare.times.current = 0;
      epic.firstObtainedRound.current = 0;
      epic.times.current = 0;
      ancient.firstObtainedRound.current = 0;
      ancient.times.current = 0;
      mythical.firstObtainedRound.current = 0;
      mythical.times.current = 0;
      for (let round = 1; round <= this.times; round++) {
        let rarityIndex = this.getRandomBreedingRarityIndex();
        let rarityModel = this.rarities[rarityIndex];
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
      for (let index = 0; index < this.rarities.length; index++) {
        let rarityModel = this.rarities[index];
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
    getRandomBreedingRarityIndex() {
      let randomizedNumber = this.getRandomInt(1, 10000);
      let commonZone = this.percentResult[0] * 100;
      let uncommonZone = this.percentResult[1] * 100 + commonZone;
      let rareZone = this.percentResult[2] * 100 + uncommonZone;
      let epicZone = this.percentResult[3] * 100 + rareZone;
      let ancientZone = this.percentResult[4] * 100 + epicZone;
      if (randomizedNumber <= commonZone) {
        return this.rarityIndex.common;
      } else if (randomizedNumber <= uncommonZone) {
        return this.rarityIndex.uncommon;
      } else if (randomizedNumber <= rareZone) {
        return this.rarityIndex.rare;
      } else if (randomizedNumber <= epicZone) {
        return this.rarityIndex.epic;
      } else if (randomizedNumber <= ancientZone) {
        return this.rarityIndex.ancient;
      } else {
        return this.rarityIndex.mythical;
      }
    },
    getRandomInt(min, max) {
      return Math.floor(Math.random() * max) + min;
    },
    getPercent(number, total) {
      return ((number / total) * 100).toFixed(2);
    },
  },
};
</script>

<style scoped></style>
