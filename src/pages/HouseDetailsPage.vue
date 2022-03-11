<template>
  <div class="container">
    <div class="row mt-3 bg-light shadow p-4">
      <div class="col-md-6">
        <img :src="house.imgUrl" class="img-fluid" alt="" srcset="" />
      </div>
      <div class="col-md-6">
        <h1>{{ house.bedrooms }} | {{ house.bathrooms }}</h1>
        <h2>{{ house.year }}</h2>
        <p>{{ house.description }}</p>
        <b>{{ house.price }}</b>
      </div>
    </div>
  </div>
</template>

<script>
import { computed, onMounted } from "@vue/runtime-core";
import { AppState } from "../AppState";
import { logger } from "../utils/Logger";
import { useRoute } from "vue-router";
import { housesService } from "../services/HousesService";
import Pop from "../utils/Pop";
import { Modal } from "bootstrap";

export default {
  setup() {
    const route = useRoute();
    onMounted(async () => {
      try {
        AppState.activeHouse = {};
        logger.log(route.params);
        await housesService.getById(route.params.id);
      } catch (error) {
        logger.log(error);
        Pop.toast(error.message, "error");
      }
    });
    return {
      house: computed(() => AppState.activeHouse),
      // openModal() {
      //   Modal.getOrCreateInstance(document.getElementById("form-modal")).show();
      // },
    };
  },
};
</script>

<style scoped>
</style>