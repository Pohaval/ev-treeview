<template lang="pug">
.white.mr-2.rounded-xl
  v-chip(
    color=""
    :outlined="!outlined"
    style="height: auto"
  ).py-2.text-center
    strong.mx-2.black--text {{ item.id }}
    template(v-if="Array.isArray(item.children)")
      no-v-nested-card(
        v-for="nested in item.children"
        :key="nested.id"
        :outlined="!outlined"
        :item="nested"
      )
    no-v-nested-card(
      v-else
      :item="item.children"
    )

</template>
<script>
export default {
  name: 'NoVNestedCard',
  props: {
    item: { type: Object, required: true },
    outlined: { type: Boolean, default: true,}
  },
  computed: {
    getRandomColor() {
      var letters = '0123456789ABCDEF';
      var color = '#';
      for (var i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      console.log(color);
      return color;
    },
  },
};
</script>
