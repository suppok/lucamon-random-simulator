<template>
  <div>
    <div class="row">
      <div class="btn-group btn-group-toggle" data-toggle="buttons">
        <label
          class="btn btn-primary"
          :class="{ active: type === types.hatching }"
          @click="type = types.hatching"
        >
          Hatching
        </label>
        <label
          class="btn btn-primary"
          :class="{ active: type === types.breeding }"
          @click="type = types.breeding"
        >
          Breeding
        </label>
      </div>
    </div>
    <div class="card shadow">
      <div v-if="type === types.hatching">
        <Hatching />
      </div>
      <div v-if="type === types.breeding">
        <Breeding />
      </div>
    </div>
  </div>
</template>

<script>
import Hatching from './components/Hatching';
import Breeding from './components/Breeding';
export default {
  name: 'App',
  components: {
    Hatching,
    Breeding,
  },
  data() {
    return {
      types: {
        hatching: 1,
        breeding: 2,
      },
      type: 1,
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
body::-webkit-scrollbar {
  display: none;
}
</style>
