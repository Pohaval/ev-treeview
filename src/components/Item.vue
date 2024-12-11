<template lang="pug">
  v-list-item(
    :value="item"
    :input-value="!!selected"
    link
    @click="toggle"
  )
    template(v-slot:default="{ active }")
      v-list-item-action: v-checkbox(:input-value="active" color="purple")
      v-list-item-title {{ item[itemText] }}
</template>
<script>
export default {
  name: 'EvTreeView',
  props: {
    value: {
      type: Boolean,
      default: null,
    },
    item: {
      type: [Object, String],
      required: true,
    },
    // itemValue: {
    //   type: String,
    //   required: true,
    // },
    itemText: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      selected: false
    }
  },
  watch: {
    value: {
      immediate: true,
      handler(val) {
        this.selected = val;
      },
    }
  },
  methods: {
    toggle() {
      this.selected = !this.selected;
      this.$emit('toggle', this.selected);
    },
  },
};
</script>
