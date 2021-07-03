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

        <div class="row mb-5">
          <span class="offset-6 col-6 text-end">Total {{ totalPercent }}%</span>
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
      <div class="offset-4 col-4 text-center">
        <h1>Result {{ totalTime }} hours with {{ totalCost }} Baht</h1>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Time',
  data() {
    return {
      eggPrice: 1000,
      totalTime: 0,
      totalCost: 0,
      totalPercent: 100,
      commonPercent: 50,
      uncommonPercent: 40,
      rarePercent: 9,
      epicPercent: 1,
      hasMythical: false,
      breedingTime: 2,
      commonHatchingTime: 12,
      uncommonHatchingTime: 24,
      rareHatchingTime: 36,
      epicHatchingTime: 48,
      ancientHatchingTime: 60,
      mythicalHatchingTime: 120,
      rarityIndex: {
        common: 0,
        uncommon: 1,
        rare: 2,
        epic: 3,
        ancient: 4,
        mythical: 5,
      },
      newLucamonFromBreeding: {
        rarity: 0,
        remainingTime: 0,
      },
      hatchingCountdown: 0,
      breedingCountdown: 0,
      availableLucamons: [],
      hasEgg: false,
      hatchingRarity: 0,
      isBreeding: false,
      isHatching: false,
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
      if (this.totalPercent !== 100) {
        return alert('ใส่ให้ครบ 100% ไอเหี้ย');
      }

      let lucamons = this.availableLucamons;
      while (!this.hasMythical) {
        if (this.hatchingCountdown === 0 && this.isHatching) {
          this.addNewLucamonFromHatching();
          this.isHatching = false;
        }
        if (this.breedingCountdown === 0 && this.isBreeding) {
          this.addNewLucamonFromBreeding();
          this.isBreeding = false;
        }
        if (this.hasMythical) {
          break;
        }
        if (this.availableLucamons.length >= 2) {
          if (!this.isBreeding) {
            this.breed(lucamons.length - 1, lucamons.length - 2);
          }
        } else {
          if (!this.hasEgg) {
            this.buyEgg();
          }
          if (!this.isHatching) {
            this.hatch();
          }
        }
        if (this.breedingCountdown > 0) {
          this.breedingCountdown--;
        }
        if (this.hatchingCountdown > 0) {
          this.hatchingCountdown--;
        }
        this.totalTime++;
      }
    },
    breed(lucamonIndex1, lucamonIndex2) {
      let lucamons = this.availableLucamons;
      let lucamon1 = lucamons[lucamonIndex1];
      let lucamon2 = lucamons[lucamonIndex2];
      lucamon1.breedingLeft--;
      lucamon2.breedingLeft--;
      this.isBreeding = true;
      this.calculateBreedingPercent(lucamon1, lucamon2);
      this.randomlyBreed();
      if (lucamon1.breedingLeft === 0) {
        lucamons.splice(lucamonIndex1, 1);
      }
      if (lucamon2.breedingLeft === 0) {
        lucamons.splice(lucamonIndex2, 1);
      }
    },
    buyEgg() {
      this.totalCost += this.eggPrice;
      this.hasEgg = true;
    },
    randomlyBreed() {
      this.newLucamonFromBreeding.rarity = this.getRandomBreedingRarityIndex();
      this.breedingCountdown = this.breedingTime;
    },
    hatch() {
      let rarityIndex = this.getRandomRarityIndex();
      this.hatchingRarity = rarityIndex;
      this.hatchingCountdown = this.getHatchingTime(rarityIndex);
      this.isHatching = true;
      this.hasEgg = false;
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
        rarity: this.hatchingRarity,
        breedingLeft: 6,
      };
      this.availableLucamons.push(newLucamon);
    },
    addNewLucamonFromBreeding() {
      let newLucamon = {
        rarity: this.newLucamonFromBreeding.rarity,
        breedingLeft: 6,
      };
      this.availableLucamons.push(newLucamon);
      if (newLucamon.rarity === this.rarityIndex.mythical) {
        this.hasMythical = true;
      }
    },
    calculateBreedingPercent(lucamon1, lucamon2) {
      let firstTotalWeight = this.rarities[lucamon1.rarity].weight;
      let secondTotalWeight = this.rarities[lucamon2.rarity].weight;
      let totalWeight = 0;
      for (let i = 0; i < 6; i++) {
        let firstWeight =
          (firstTotalWeight * this.weightList[lucamon1.rarity][i]) / 100;
        let secondWeight =
          (secondTotalWeight * this.weightList[lucamon2.rarity][i]) / 100;
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
