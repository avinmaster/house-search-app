<template>
  <div class="results">
    <div v-for="(result, i) in results" :key="i" class="result">
      <h2>{{ result.name }}</h2>
      <p>{{ result.price }} $</p>
      <p><i class="fa fa-bed"></i> Bedrooms - {{ result.bedrooms }}</p>
      <p><i class="fa fa-bath"></i> Bathrooms - {{ result.bathrooms }}</p>
      <p><i class="fa fa-boxes"></i> Storeys - {{ result.storeys }}</p>
      <p><i class="fa fa-car"></i> Garages - {{ result.garages }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "ResultsComponent",
  props: ["results"],
  // data() {
  //   return { };
  // },
};
</script>

<style scoped lang="scss">
.results {
  display: flex;
  flex-wrap: wrap;
  padding: 0 4%;
}
.result {
  min-width: 300px;
  margin: 50px auto;
  padding: 40px;
  background: #ffffff;
  box-shadow: 0 7px 20px rgba(0, 0, 0, 0.06);

  p {
    margin-top: 5px;
  }
}
</style>
