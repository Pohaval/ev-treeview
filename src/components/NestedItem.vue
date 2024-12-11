<template lang="pug">
v-list-group.px-0(
  color="null"
  light
)
  template(v-slot:activator)
    v-list-item-action
      v-checkbox(
        light
        color="purple"
        :input-value="allSelected"
        :indeterminate="indeterminate"
        @click.stop="onNestedCheckbox"
      )
    v-list-item-title {{ item[itemText]}}
  v-sheet(width="100%").pl-2
    template(
      v-for="(nested, idx) in item.children"
    )
      nested-item(
        ref="nested"
        v-if="nested.children && nested.children.length"
        :value="nestedValue(nested)"
        :item="nested"
        :multiple="multiple"
        :key="`${idx}-group`"
        @input="(val) => onInput(val, nested)"
      )

      item(
        v-else
        :value="checkSelected(nested)"
        :item="nested"
        :item-text="itemText"
        :key="`${idx}`"
        @toggle="onToggle(nested, $event)"
      )
        template(v-slot:default="{ active }")
          v-list-item-action: v-checkbox(:input-value="active" color="purple") mdi-close
          v-list-item-title {{ item.title }}
</template>
<script>
import Item from "./Item.vue";

export default {
  name: 'NestedItem',
  components: {
    Item,
  },
  props: {
    value: {
      type: Object,
      default: null,
    },
    multiple: {
      type: Boolean,
      default: false,
    },
    item: {
      type: [Object, String],
      required: true,
    },
    itemText: {
      type: String,
      default: 'text',
    },
    itemValue: {
      type: String,
      default: 'id',
    },
  },
  data() {
    return {
      selected: null,
    };
  },
  computed: {
    allSelected() {
      if (this.multiple) {
        return !!this.selected && (this.selected.length === this.item.children.length);
      }
      return !!this.selected
    },
    indeterminate() {
      if (this.multiple) {
        return this.selected && (this.selected.length > 0 && this.selected.length < this.item.children.length);
      }
      return false;
    }
  },
  watch: {
    value: {
      immediate: true,
      deep: true,
      handler(val) {
        if (!val) this.selected = null;
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
          return item;
        }
        return null;
      }
      return null;
    },
    onToggle(item, val) {
      const { getTrueVal, getFalseVal } = this;
      if (val) {
        this.selected = getTrueVal(item);
      } else this.selected = getFalseVal(item);
      this.returnSelected(this.selected);
    },

    returnSelected(selected) {
      if (selected) {
        this.$emit('input', {
          ...this.item,
          children: selected,
        });
      } else {
        this.$emit('input', null);
      }
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
      if (multiple) {
        const output = selected.filter((cur) => cur[this.itemValue] !== item[this.itemValue])
        return output.length ? output : null;
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

    async onNestedCheckbox() {
      if (this.allSelected) this.resetAll();
      else this.selectAll()
      this.returnSelected(this.selected);
    },
    selectAll() {
      this.selected = this.item.children;
      this.$refs?.nested?.forEach(({ selectAll }) => selectAll());
    },
    resetAll() {
      this.selected = null;
      this.$refs?.nested?.forEach(({ resetAll }) => resetAll());
    },

    onInput(item, curItem) {
      const { getNestedTrueVal, getFalseVal } = this;
      if (item) this.selected = getNestedTrueVal(item);
      else this.selected = getFalseVal(curItem);
      this.returnSelected(this.selected);
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
  },
};
</script>
