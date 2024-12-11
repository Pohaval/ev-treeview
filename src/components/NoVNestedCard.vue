<template lang="pug">
v-card
  v-card-title {{ item.id }}
  v-card-text(v-if="Array.isArray(item.children)")
    no-v-nested-card(
      v-for="nested in item.children"
      :key="nested.id"
      :item="nested"
    )
  v-card-text(v-else)
    no-v-nested-card(
      :item="item.children"
    )

</template>
<script>
export default {
  name: 'NoVNestedCard',
  props: {
    item: { type: Object, required: true,}
  },
};
</script>
