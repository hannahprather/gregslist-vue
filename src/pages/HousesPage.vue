<template>
  <div class="container-fluid">
    <div class="row">
      <div v-for="h in houses" :key="h.id" class="col-4 p-4 mb-2">
        <House :house="h" />
      </div>
    </div>
  </div>
</template>

<script>
import { computed, onMounted } from "@vue/runtime-core";
import { logger } from "../utils/Logger";
import { housesService } from "../services/HousesService";
import { AppState } from "../AppState";

export default {
  setup() {
    onMounted(async () => {
      try {
        await housesService.getAll();
      } catch (error) {
        logger.error(error);
      }
    });
    return {
      houses: computed(() => AppState.houses),
    };
  },
};
</script>


<style>
</style>