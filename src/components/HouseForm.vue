<template>
  <form>
    <div class="form-group">
      <label for="bedrooms" class="">Bedrooms:</label>
      <select
        v-model="editable.bedrooms"
        name="bedrooms"
        id="bedrooms"
        required
        class="form-control"
      ></select>
    </div>
    <div class="form-group">
      <label for="bathrooms" class="">Bathrooms:</label>
      <input
        v-model="editable.bathrooms"
        placeholder="Bathrooms"
        type="text"
        class="form-control"
        name="Bathrooms"
        id="bathrooms"
        required
      />
    </div>
    <div class="form-group">
      <label for="price" class="">Price:</label>
      <input
        v-model="editable.price"
        placeholder="Price"
        type="number"
        class="form-control"
        name="price"
        id="price"
        min="0"
        max="9999999"
      />
    </div>
    <div class="form-group">
      <label for="year" class="">year:</label>
      <input
        v-model="editable.year"
        placeholder="year"
        type="year"
        class="form-control"
        name="year"
        id="year"
      />
    </div>
    <div class="form-group">
      <label for="description" class="">description:</label>
      <textarea
        v-model="editable.description"
        placeholder="Description"
        type="text"
        class="form-control"
        name="description"
        id="description"
        rows="5"
      ></textarea>
    </div>
    <div class="form-group">
      <label for="img" class="">img:</label>
      <input
        v-model="editable.imgUrl"
        placeholder="https://imgurl.com"
        type="url"
        class="form-control"
        name="img"
        id="img"
        required
      />
    </div>
    <div class="d-flex justify-content-between my-3">
      <button
        type="button"
        data-bs-dismiss="modal"
        aria-label="Close"
        class="btn text-dark lighten-20 text-uppercase selectable"
      >
        <b> cancel </b>
      </button>
      <button
        v-if="!houseData.id"
        @click="createHouse"
        type="button"
        class="btn btn-success text-dark text-uppercase selectable"
      >
        <b> Create House </b>
      </button>
      <button
        v-else
        @click="editHouse"
        type="button"
        class="btn btn-info text-warning text-uppercase selectable"
      >
        <b> Edit House </b>
      </button>
    </div>
  </form>
</template>


<script>
import { AppState } from "../AppState";
import { computed, reactive, onMounted, ref, watchEffect } from "vue";
import { logger } from "../utils/Logger";
import { housesService } from "../services/HousesService";
import Pop from "../utils/Pop";
import { Modal } from "bootstrap";
import { useRouter } from "vue-router";
export default {
  props: {
    houseData: {
      type: Object,
      required: false,
      default: {},
    },
  },
  setup(props) {
    const editable = ref({});
    const router = useRouter();
    watchEffect(() => {
      logger.log("change happened re-running watch effect");
      editable.value = props.houseData;
    });
    return {
      editable,
      async createHouse() {
        try {
          logger.log("editable before service", editable.value);
          // NOTE we save the car response from the server here so we can push later
          let newHouse = await houseService.create(editable.value);
          editable.value = {};
          Modal.getOrCreateInstance(
            document.getElementById("form-modal")
          ).hide();
          router.push({ name: "houseDetails", params: { id: newHouse.id } });
        } catch (error) {
          logger.error(error);
          Pop.toast(error.message, "error");
        }
      },
    };
  },
};
</script>
<style></style>