<template>
  <span>
    <button v-if="pageNumber > 0" type="button">
      <span v-if="pageNumber === page" style="color: aquamarine">
        {{ pageNumber }}
      </span>
      <span v-else>
        {{ pageNumber }}
      </span>
    </button>
  </span>
</template>

<script>
export default {
  name: "ButtonPage",
  props: ["pageNumber", "page"],
};
</script>

<style>
</style>