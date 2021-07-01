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
      type: 2,
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
