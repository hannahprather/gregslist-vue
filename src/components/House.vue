<template>
  <router-link :to="{ name: 'HouseDetails', params: { id: house.id } }">
    <div class="house p-3 bg-light rounded shadow selectable">
      <div>
        <img class="img-fluid" :src="house.imgUrl" alt="" />
      </div>
      <h3 class="text-center">
        {{ house.year }} | {{ house.bedrooms }}| {{ house.bathrooms }}
      </h3>
      <div>
        <p>{{ house.description }}</p>
        <p>{{ house.price }}</p>
        <p>{{ house.year }}</p>
      </div>
    </div>
  </router-link>
</template>

<script>
export default {
  props: {
    house: {
      type: Object,
      required: true,
    },
  },
  setup() {
    return {};
  },
};
</script>

<style>
</style>