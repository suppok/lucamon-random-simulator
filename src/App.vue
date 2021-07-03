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
        <label
          class="btn btn-primary"
          :class="{ active: type === types.growth }"
          @click="type = types.growth"
        >
          Growth
        </label>
        <label
          class="btn btn-primary"
          :class="{ active: type === types.time }"
          @click="type = types.time"
        >
          Time
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
      <div v-if="type === types.growth">
        <Growth />
      </div>
      <div v-if="type === types.time">
        <Time />
      </div>
    </div>
  </div>
</template>

<script>
import Hatching from './components/Hatching';
import Breeding from './components/Breeding';
import Growth from './components/Growth';
import Time from './components/Time';
export default {
  name: 'App',
  components: {
    Hatching,
    Breeding,
    Growth,
    Time,
  },
  data() {
    return {
      types: {
        hatching: 1,
        breeding: 2,
        growth: 3,
        time: 4,
      },
      type: 4,
    };
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
