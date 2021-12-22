<template>
  <div class="wrapper">
    <button
      class="filters-btn"
      @click="$emit('toggleFilters')"
      title="Open filters"
    >
      <i class="fa fa-sliders-h"></i>
    </button>
    <input
      type="text"
      placeholder="Search here"
      class="search-input"
      @input="$emit('input', $event.target.value)"
      v-on:keyup.13="$emit('submit')"
      title="Search here"
    />
    <button class="search-btn" @click="$emit('submit')" title="Click to search">
      <i class="fa fa-search"></i>
    </button>
  </div>
</template>

<script>
export default {
  name: "SearchComponent",
  props: {
    loading: {
      type: Boolean,
    },
  },
};
</script>

<style scoped lang="scss">
.wrapper {
  width: 70%;
  margin: 0 15%;
  height: 50px;
  background: #f1dfe3;
  border-radius: 5px;
  box-shadow: 0 7px 20px rgba(0, 0, 0, 0.04);
}

.search-input {
  float: left;
  background: #fafafa;
  border: 1px solid #eeeeee;
  height: 50px;
  padding: 0 15px;
  margin-left: 5px;
  width: calc(100% - 105px);
  font-size: 20px;
  color: #3c3f50;
  font-family: "Segoe UI Light", "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  transition: all 0.3s;

  &:focus {
    background: #ffffff;
  }
}

.search-btn,
.filters-btn {
  font-size: 20px;
  float: right;
  height: 50px;
  width: 50px;
  background: #fafafa;
  border-radius: 0 5px 5px 0;
  border: 1px solid #eeeeee;
  border-left: none;
  color: #464958;
  transition: all 0.3s;

  &:hover,
  &:focus {
    background: #ffffff;
  }

  &:active {
    background: #f8f8f8;
  }
}

.filters-btn {
  float: left;
  border-left: 1px solid #eeeeee;
  // border-right: none;
  border-radius: 5px 0 0 5px;
}
</style>
