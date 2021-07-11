<template>
  <div>
    <div class="card-header">
      <div class="offset-4 col-4">
        <h1 class="text-center">Time</h1>
        <div class="row mb-2">
          <label class="col-4"><b>Rarity</b></label>
          <label class="offset-1 col-3 text-center"><b>Percent</b></label>
          <label class="offset-1 col-3 text-center"><b>Time</b></label>
        </div>
        <div class="row mb-2">
          <label class="col-4">Common</label>
          <input
            v-model="commonPercent"
            type="number"
            class="offset-1 col-3 text-end"
          />
          <input
            v-model="commonHatchingTime"
            type="number"
            class="offset-1 col-3 text-end"
          />
        </div>
        <div class="row mb-2">
          <label class="col-4">Uncommon</label>
          <input
            v-model="uncommonPercent"
            type="number"
            class="offset-1 col-3 text-end"
          />
          <input
            v-model="uncommonHatchingTime"
            type="number"
            class="offset-1 col-3 text-end"
          />
        </div>
        <div class="row mb-2">
          <label class="col-4">Rare</label>
          <input
            v-model="rarePercent"
            type="number"
            class="offset-1 col-3 text-end"
          />
          <input
            v-model="rareHatchingTime"
            type="number"
            class="offset-1 col-3 text-end"
          />
        </div>
        <div class="row mb-2">
          <label class="col-4">Epic</label>
          <input
            v-model="epicPercent"
            type="number"
            class="offset-1 col-3 text-end"
          />
          <input
            v-model="epicHatchingTime"
            type="number"
            class="offset-1 col-3 text-end"
          />
        </div>
        <div class="row mb-2">
          <label class="col-4">Ancient</label>
          <input
            v-model="ancientPercent"
            type="number"
            class="offset-1 col-3 text-end"
            disabled
          />
          <input
            v-model="ancientHatchingTime"
            type="number"
            class="offset-1 col-3 text-end"
          />
        </div>
        <div class="row mb-2">
          <label class="col-4">Mythical</label>
          <input
            v-model="mythicalPercent"
            type="number"
            class="offset-1 col-3 text-end"
            disabled
          />
          <input
            v-model="mythicalHatchingTime"
            type="number"
            class="offset-1 col-3 text-end"
          />
        </div>

        <div class="row mb-2">
          <span class="offset-6 col-6 text-end">Total {{ totalPercent }}%</span>
        </div>
        <div class="row mb-2">
          <label class="col-4">Egg Price</label>
          <input
            v-model="eggPrice"
            type="number"
            class="offset-5 col-3 text-end"
          />
        </div>
        <div class="row mb-2">
          <label class="col-4">Breeding Time</label>
          <input
            v-model="breedingTime"
            type="number"
            class="offset-5 col-3 text-end"
          />
        </div>
        <div class="row mb-2">
          <label class="col-4">Breeding Limit</label>
          <input
            v-model="breedingLimit"
            type="number"
            class="offset-5 col-3 text-end"
          />
        </div>
        <div class="row mb-2">
          <label class="col-4">Breeding Chance Min</label>
          <input
            v-model="breedingChanceMin"
            type="number"
            class="offset-5 col-3 text-end"
          />
        </div>
        <div class="row mb-2">
          <label class="col-4">Breeding Chance Max</label>
          <input
            v-model="breedingChanceMax"
            type="number"
            class="offset-5 col-3 text-end"
          />
        </div>
        <div class="row mb-2">
          <label class="col-4">Breeding Fee</label>
          <input
            v-model="breeding.fee"
            type="number"
            class="offset-5 col-3 text-end"
          />
        </div>
        <div class="row mb-5">
          <label class="col-4">Hatching Fee</label>
          <input
            v-model="hatching.fee"
            type="number"
            class="offset-5 col-3 text-end"
          />
        </div>
        <div class="row mb-2">
          <button class="btn btn-sm btn-warning col-6" @click="reset">
            Reset
          </button>
          <button
            class="btn btn-sm btn-primary col-6"
            @click="simulate"
            :disabled="totalPercent !== 100"
          >
            Simulate!
          </button>
        </div>
      </div>
    </div>
    <div class="card-body">
      <div class="offset-3 col-6">
        <h4>
          Result <br />
          Time: {{ totalTime }} hours<br />
          - {{ totalTime / 24 }} days<br />
          Costs: {{ totalCost }} Baht<br />
          - Breeding Fee: {{ totalBreed * breedingFeeNum }} Baht ({{
            totalBreed
          }}
          times)<br />
          - Hatching Fee: {{ totalHatch * hatchingFeeNum }} Baht ({{
            totalHatch
          }}
          times)<br />
          - Buying Fee: {{ totalEggBuying * eggPrice }} Baht ({{
            totalEggBuying
          }}
          times)<br />
          Lucamons: {{ allLucamons.length }}
        </h4>
        <div class="table-responsive">
          <table class="table table-bordered w-100" id="data-table">
            <thead>
              <tr>
                <th style="text-align: center; vertical-align: center">#</th>
                <th style="text-align: center">Action</th>
                <th style="text-align: center">Detail</th>
              </tr>
            </thead>
            <tbody>
              <template v-for="(log, index) in logs" :key="index">
                <tr>
                  <td>{{ log.num }}</td>
                  <td style="text-align: right">
                    {{ log.action }}
                  </td>
                  <td style="text-align: right">
                    {{ log.detail }}
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
  name: 'Time',
  data() {
    return {
      breeding: {
        countdown: 0,
        rarity: 0,
        isBreeding: false,
        chance: 0,
        fee: 45,
      },
      hatching: {
        rarity: 0,
        countdown: 0,
        isHatching: false,
        fee: 15,
      },

      breedingFees: [15, 30, 45, 90, 180],

      hasMythical: false,
      eggPrice: 600,
      totalTime: 0,
      totalCost: 0,
      totalBreed: 0,
      totalHatch: 0,
      totalEggBuying: 0,

      breedingTime: 4,
      breedingLimit: 5,
      breedingChanceMin: 60,
      breedingChanceMax: 80,

      totalPercent: 100,
      commonPercent: 50,
      uncommonPercent: 40,
      rarePercent: 9,
      epicPercent: 1,
      ancientPercent: '',
      mythicalPercent: '',

      commonHatchingTime: 10,
      uncommonHatchingTime: 20,
      rareHatchingTime: 30,
      epicHatchingTime: 40,
      ancientHatchingTime: 50,
      mythicalHatchingTime: 100,

      rarityIndex: {
        common: 0,
        uncommon: 1,
        rare: 2,
        epic: 3,
        ancient: 4,
        mythical: 5,
      },

      availableLucamons: [],
      availableEggs: [],
      allEggs: [],
      allLucamons: [],
      logs: [],

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
      weightList: [
        [60, 35, 5, 0, 0, 0],
        [30, 40, 25, 5, 0, 0],
        [20, 30, 30, 20, 0, 0],
        [15, 20, 30, 20, 15, 0],
        [10, 20, 25, 20, 15, 10],
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
    breedingChanceMinNum: function () {
      return parseInt(this.breedingChanceMin);
    },
    breedingChanceMaxNum: function () {
      return parseInt(this.breedingChanceMax);
    },
    breedingFeeNum: function () {
      return parseInt(this.breeding.fee);
    },
    hatchingFeeNum: function () {
      return parseInt(this.hatching.fee);
    },
    isMythicalAvailable: function () {
      return this.availableLucamons.find(
        (item) => item.rarity === this.rarityIndex.mythical
      );
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
      this.breeding = {
        countdown: 0,
        rarity: 0,
        isBreeding: false,
        chance: 0,
        fee: 45,
      };
      this.hatching = {
        rarity: 0,
        countdown: 0,
        isHatching: false,
        fee: 15,
      };

      this.hasMythical = false;
      this.totalTime = 0;
      this.totalCost = 0;
      this.totalBreed = 0;
      this.totalHatch = 0;
      this.totalEggBuying = 0;

      this.totalPercent = 100;
      this.ancientPercent = '';
      this.mythicalPercent = '';

      this.rarityIndex = {
        common: 0,
        uncommon: 1,
        rare: 2,
        epic: 3,
        ancient: 4,
        mythical: 5,
      };

      this.availableLucamons = [];
      this.availableEggs = [];
      this.allEggs = [];
      this.allLucamons = [];
      this.logs = [];
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
    simulate() {
      if (!this.validate()) {
        return;
      }

      while (!this.isMythicalAvailable) {
        if (this.hatching.countdown === 0 && this.hatching.isHatching) {
          this.addNewLucamonFromHatching();
          this.hatching.isHatching = false;
        }
        if (this.isMythicalAvailable) {
          break;
        }
        if (this.breeding.countdown === 0 && this.breeding.isBreeding) {
          if (this.isBreedSuccessfully()) {
            this.addNewEggFromBreeding(this.breeding.rarity);
          }
          this.breeding.isBreeding = false;
        }
        if (this.availableLucamons.length >= 2) {
          if (!this.breeding.isBreeding && !this.hatching.isHatching) {
            this.breedingStep();
          }
        }

        if (this.availableEggs.length > 0) {
          if (!this.hatching.isHatching) {
            this.hatchingStep();
          }
        }

        if (this.hasNoEggAndCantBreed()) {
          if (!this.breeding.isBreeding) {
            this.buyEgg();
          }
        }
        if (this.hasAnyEgg()) {
          if (!this.hatching.isHatching) {
            this.hatchingStep();
          }
        }

        this.reduceBreedingTime();
        this.reduceHatchingTime();
        this.addHour();
      }
    },
    hasAnyEgg() {
      return this.availableEggs.length > 0;
    },
    hasNoEggAndCantBreed() {
      return (
        this.availableLucamons.length < 2 && this.availableEggs.length === 0
      );
    },
    breedingStep() {
      this.totalBreed++;
      let lucamons = this.availableLucamons;
      lucamons.sort(this.compare);
      let lucamonIndex1 = lucamons.length - 1;
      let lucamonIndex2 = lucamons.length - 2;

      this.breeding.rarity = this.breed(lucamonIndex1, lucamonIndex2);
      let lucamon1 = lucamons[lucamonIndex1];
      let lucamon2 = lucamons[lucamonIndex2];
      let lucamon1Price = this.breedingFees[5 - lucamon1.breedingLeft];
      let lucamon2Price = this.breedingFees[5 - lucamon2.breedingLeft];
      this.totalCost += (lucamon1Price + lucamon2Price) / 2;

      lucamon1.breedingLeft--;
      lucamon2.breedingLeft--;

      this.breeding.chance = this.getBreedingChance(
        lucamon1.breedingChance,
        lucamon2.breedingChance
      );

      if (this.checkBreedingAvailability(lucamonIndex1) <= 0) {
        lucamons.splice(lucamonIndex1, 1);
      }
      if (this.checkBreedingAvailability(lucamonIndex2) <= 0) {
        lucamons.splice(lucamonIndex2, 1);
      }
      this.breeding.countdown = this.breedingTime;
      this.breeding.isBreeding = true;
    },
    getBreedingChance(breedingChance1, breedingChance2) {
      return (breedingChance1 + breedingChance2) / 2;
    },
    reduceBreedingTime() {
      if (this.breeding.countdown > 0) {
        this.breeding.countdown--;
      }
    },
    reduceHatchingTime() {
      if (this.hatching.countdown > 0) {
        this.hatching.countdown--;
      }
    },
    addHour() {
      this.totalTime++;
    },
    validate() {
      if (this.totalPercent !== 100) {
        alert('ใส่ให้ครบ 100%');
        return false;
      }

      return true;
    },
    breed(lucamonIndex1, lucamonIndex2) {
      let lucamons = this.availableLucamons;
      let lucamon1 = lucamons[lucamonIndex1];
      let lucamon2 = lucamons[lucamonIndex2];

      let percentResult = this.calculateBreedingPercent(lucamon1, lucamon2);
      let rarity = this.randomlyBreed(percentResult);
      this.addBreedingLog(lucamon1, lucamon2);
      return rarity;
    },
    checkBreedingAvailability(lucamonIndex) {
      return this.availableLucamons[lucamonIndex].breedingLeft;
    },
    buyEgg() {
      this.totalEggBuying++;
      this.totalCost += this.eggPrice;
      let rarityIndex = this.getRandomRarityIndex();
      let newEgg = {
        rarity: rarityIndex,
        hatchingTime: this.getHatchingTime(rarityIndex),
      };
      this.availableEggs.push(newEgg);
      this.availableEggs.sort(this.compare);
      this.allEggs.push(newEgg);
      this.addBuyingLog();
    },
    randomlyBreed(percentResult) {
      return this.getRandomBreedingRarityIndex(percentResult);
    },
    //User don't know egg rarity, therefore this logic will be the best egg first
    hatchingStep() {
      this.totalHatch++;
      this.totalCost += this.hatchingFeeNum;
      let eggs = this.availableEggs;
      eggs.sort(this.compare);
      let hatchedEgg = eggs[eggs.length - 1];
      this.hatching.rarity = hatchedEgg.rarity;
      this.hatching.countdown = hatchedEgg.hatchingTime;
      this.hatching.isHatching = true;
      eggs.splice(eggs.length - 1, 1);
      this.addHatchingLog(hatchedEgg);
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
    getRandomBreedingRarityIndex(percentResult) {
      let randomizedNumber = this.getRandomInt(1, 10000);
      let commonZone = percentResult[0] * 100;
      let uncommonZone = percentResult[1] * 100 + commonZone;
      let rareZone = percentResult[2] * 100 + uncommonZone;
      let epicZone = percentResult[3] * 100 + rareZone;
      let ancientZone = percentResult[4] * 100 + epicZone;
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
    getHatchingTime(rarityIndex) {
      switch (rarityIndex) {
        case this.rarityIndex.common:
          return this.commonHatchingTime;
        case this.rarityIndex.uncommon:
          return this.uncommonHatchingTime;
        case this.rarityIndex.rare:
          return this.rareHatchingTime;
        case this.rarityIndex.epic:
          return this.epicHatchingTime;
        case this.rarityIndex.ancient:
          return this.ancientHatchingTime;
        case this.rarityIndex.mythical:
          return this.mythicalHatchingTime;
      }
    },
    addNewLucamonFromHatching() {
      let newLucamon = {
        rarity: this.hatching.rarity,
        breedingLeft: this.breedingLimit,
        breedingChance: this.getRandomInt(
          this.breedingChanceMinNum,
          this.breedingChanceMaxNum - this.breedingChanceMinNum
        ),
      };
      this.availableLucamons.push(newLucamon);
      this.availableLucamons.sort(this.compare);
      this.allLucamons.push(newLucamon);
      if (newLucamon.rarity === this.rarityIndex.mythical) {
        this.hasMythical = true;
      }
    },
    addNewEggFromBreeding(rarity) {
      let hatchingTime = this.getHatchingTime(rarity);
      let newEgg = {
        rarity: rarity,
        hatchingTime: hatchingTime,
      };
      this.availableEggs.push(newEgg);
      this.availableEggs.sort(this.compare);
      this.allEggs.push(newEgg);
    },
    calculateBreedingPercent(lucamon1, lucamon2) {
      let percentResult = [0, 0, 0, 0, 0, 0];
      let firstTotalWeight = this.rarities[lucamon1.rarity].weight;
      let secondTotalWeight = this.rarities[lucamon2.rarity].weight;
      let totalWeight = 0;
      for (let i = 0; i < 6; i++) {
        let firstWeight =
          (firstTotalWeight * this.weightList[lucamon1.rarity][i]) / 100;
        let secondWeight =
          (secondTotalWeight * this.weightList[lucamon2.rarity][i]) / 100;
        percentResult[i] = firstWeight + secondWeight;
        totalWeight += percentResult[i];
      }
      for (let i = 0; i < 6; i++) {
        percentResult[i] = this.getPercent(percentResult[i], totalWeight);
      }

      return percentResult;
    },
    getRandomInt(min, max) {
      return Math.floor(Math.random() * max) + min;
    },
    getPercent(number, total) {
      return ((number / total) * 100).toFixed(2);
    },
    compare(a, b) {
      if (a.rarity < b.rarity) {
        return -1;
      }
      if (a.rarity > b.rarity) {
        return 1;
      }
      return 0;
    },
    getRarityName(index) {
      return this.rarities[index].name;
    },
    addBreedingLog(lucamon1, lucamon2) {
      let num = this.logs.length + 1;
      let rarity1 = this.getRarityName(lucamon1.rarity);
      let rarity2 = this.getRarityName(lucamon2.rarity);
      this.logs.push({
        num: num,
        action: 'Breeding',
        detail: rarity1 + ' + ' + rarity2,
      });
    },
    addHatchingLog(egg) {
      let num = this.logs.length + 1;
      let rarity = this.getRarityName(egg.rarity);
      this.logs.push({
        num: num,
        action: 'Hatching',
        detail: rarity,
      });
    },
    addBuyingLog() {
      let num = this.logs.length + 1;
      this.logs.push({
        num: num,
        action: 'Buying',
        detail: '',
      });
    },
    isBreedSuccessfully() {
      let chance = this.breeding.chance * 100;
      let randomNumber = this.getRandomInt(1, 10000);
      if (randomNumber <= chance) {
        return true;
      } else {
        return false;
      }
    },
  },
};
</script>

<style scoped></style>
