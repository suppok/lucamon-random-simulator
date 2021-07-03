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
          <label class="col-6" for="times">Times</label>
          <input
            v-model="times"
            type="number"
            id="times"
            class="offset-3 col-3 text-end"
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
    <div class="card-body"></div>
  </div>
</template>

<script>
export default {
  name: 'Time',
  data() {
    return {
      totalPercent: 100,
      commonPercent: 50,
      uncommonPercent: 40,
      rarePercent: 9,
      epicPercent: 1,
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
      hatchingCountdown: 0,
      breedingCountdown: 0,
      availableLucamons: [[], [], [], [], [], []],
      eggs: [],
      isBreeding: [],
      isHatching: [],
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
    totalAvailableLucamons: function () {
      let total = 0;
      for (const list of this.availableLucamons) {
        total += list.length;
      }

      return total;
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
      let hour = 0;
      while (true)
        if (this.totalAvailableLucamons >= 2) {
          if (!this.isBreeding) {
          }
        }
    },
  },
};
</script>

<style scoped></style>
