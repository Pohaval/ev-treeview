<template lang="pug">
v-list(
  :expand="expand"
)
  template(
    v-for="(item, key) in items"
  )
    nested-item(
      v-if="item.children && item.children.length"
      :value="nestedValue(item)"
      :item="item"
      group
      v-bind="nestedAttrs"
      :key="`${key}-group`"
      @input="(val) => onInput(val, item)"
    )
    item(
      v-else
      :value="checkSelected(item)"
      :item="item"
      :item-text="itemText"
      :key="`${key}`"
      @toggle="(val) => onToggle(item, val)"
    )
</template>
<script>
import Item from "./Item.vue";
import NestedItem from "./NestedItem.vue";

export default {
  name: 'EvTreeView',
  components: {
    Item,
    NestedItem,
  },
  props: {
    value: {
      type: [Array, Object],
      default: null,
    },
    items: {
      type: Array,
      required: true,
    },
    itemValue: {
      type: String,
      default: 'id',
    },
    itemText: {
      type: String,
      default: 'text',
    },
    returnObject: {
      type: Boolean,
      default: false,
    },
    multiple: {
      type: Boolean,
      default: false,
    },
    expand: {
      type: Boolean,
      default: false,
    },
    nestedAttrs: {
      type: Object,
      default: () => ({ multiple: false }),
    },
  },

  data() {
    return {
      selected: null,
      extendedItems: [],
    };
  },

  watch: {
    value: {
      deep: true,
      immediate: true,
      handler(val) {
        this.selected = val;
      },
    },
    selected: {
      deep: true,
      handler(val) {
        this.$emit('input', val);
      },
    },
  },
  methods: {
    nestedValue(item) {
      if (this.selected) {
        if (this.multiple) {
          const idx = this.selected.findIndex((el) => el[this.itemValue] === item[this.itemValue])
          if (idx !== -1) return this.selected[idx]
        }
        if (this.selected[this.itemValue] === item[this.itemValue]) {
          return this.selected;
        }
        return null;
      }
      return null;
    },

    checkSelected(item) {
      const { selected, multiple } = this;
      if (multiple) {
        return selected && selected.some((el) => item[this.itemValue] === el[this.itemValue]);
      } else {
        return selected && selected[this.itemValue] === item[this.itemValue];
      }
    },
    onInput(item, curItem) {
      const { getNestedTrueVal, getFalseVal } = this;
      if (item) this.selected = getNestedTrueVal(item);
      else this.selected = getFalseVal(curItem);
    },

    getNestedTrueVal(item) {
      const { multiple, selected } = this;
      if (multiple) {
        const output = [...selected || []];
        const idx = output.findIndex((el) => item[this.itemValue] === el[this.itemValue])
        if (idx === -1) output.push(item);
        else output.splice(idx, 1, item);
        return output;
      }
      return item;
    },
    onToggle(item, val) {
      const { getTrueVal, getFalseVal } = this;
      if (val) this.selected = getTrueVal(item);
      else this.selected = getFalseVal(item);
    },

    getTrueVal(item) {
      const { multiple, selected } = this;
      if (multiple) {
        const output = [...selected || []];
        output.push(item);
        return output;
      }
      return item;
    },
    getFalseVal(item) {
      const { multiple, selected } = this;
      if (multiple) return selected.filter((cur) => cur[this.itemValue] !== item[this.itemValue]);
      else return null;
    },
  },
};
</script>

<style>

</style>
