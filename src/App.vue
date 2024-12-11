<template lang="pug">
  v-app()
    v-app-bar(
      app
      color="purple"
      flat
      dark
    )
      v-progress-linear(
        color="purple"
        height="64"
        value="100"
        striped
      )
        .text-h5(
          style="letter-spacing: 1.4px !important"
        ): strong Ev-TreeView
    v-main
      v-card(width="600")
        ev-treeview(
          v-model="selected"
          expand
          multiple
          :items="items"
          :nestedAttrs=`{
            multiple: true,
          }`
        )

        v-card
          v-card-text
            v-chip-group
            template(v-if="Array.isArray(selected)")
              NoVNestedCard.mb-2(
                v-for="item in selected"
                :key="item.id"
                cols="5" md="3"
                :item="item"
              )
            NoVNestedCard(v-else-if="selected" :item="selected")


</template>

<script>
import EvTreeview from './components/EvTreeview.vue'
import NoVNestedCard from './components/NoVNestedCard.vue'

export default {
  name: 'App',
  components: { EvTreeview, NoVNestedCard },
  data() {
    return {
      selected: [
        {
          id: 1,
          text: 'Hello',
          children: [],
        },
        {
          id: 3,
          text: 'Hello World',
          opt: {

          },
          children: [
            {
              id: 4,
              text: 'Nested Hello World',
              children: [],
            },

          ],
        },
        {
          id: 6,
          text: "Hello Worlds",
          children: [
            {
              id: 8,
              text: "Nested Hello Worlds",
              children: [
                {
                  id: 1,
                  text: "Nested nested Hello World",
                  children: []
                },
                {
                  id: 7,
                  text: 'Nested nested Hello World',
                  children: [],
                },
              ],
            },
            {
              id: 7,
              text: 'Nested Hello World',
              children: [],
            },
          ],
        }
      ],
      items: [
        {
          id: 1,
          text: 'Hello',
          children: [],
        },
        {
          id: 2,
          text: 'Hello World',
          children: [],
        },
        {
          id: 3,
          text: 'Hello World',
          opt: {

          },
          children: [
            {
              id: 4,
              text: 'Nested Hello World',
              children: [],
            },
            {
              id: 5,
              text: 'Nested Hello World',
              children: [],
            },
          ],
        },
        {
          id: 6,
          text: 'Hello Worlds',
          children: [
            {
              id: 7,
              text: 'Nested Hello World',
              children: [],
            },
            {
              id: 8,
              text: 'Nested Hello Worlds',
              children: [
                {
                  id: 1,
                  text: 'Nested nested Hello World',
                  children: [],
                },
                {
                  id: 6,
                  text: 'Nested nested Hello World',
                  children: [],
                },
                {
                  id: 7,
                  text: 'Nested nested Hello World',
                  children: [],
                },
              ],
            },
          ],
        },
      ],
    };
  },
};
</script>
