<template>
  <div class="map">
    <h3>Карта офиса</h3>

    <div v-if="!isLoading" class="map-root">
      <MapSVG ref="svg">
        <g v-click-outside="hide">
          <Table
            @onTableClick="handleClick"
            v-for="(table, i) in tables"
            :key="i"
            :table="table"
            :legend="legend"
          />
        </g>
      </MapSVG>
    </div>
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import MapSVG from "../assets/images/map.svg";
import Table from "./Table.vue";

import peoples from "../assets/data/people.json";
import ClickOutside from "vue-click-outside";

export default {
  components: {
    MapSVG,
    Table,
  },
  props: {
    tables: {
      type: Array,
      default: () => [],
    },
    legend: {
      type: Array,
      default: () => [],
    },
    person: Object,
    isUserOpened: Boolean,
  },
  data() {
    return {
      isLoading: false,
    };
  },
  methods: {
    hide() {
      this.$emit("update:isUserOpened", false);
    },
    handleClick(event) {
      this.$emit("update:isUserOpened", true);
      const tableId = event.currentTarget.getAttribute("id");

      const person = peoples.find(
        (people) => people.tableId === Number(tableId)
      );
      if (person) {
        this.$emit("update:person", person);
      }
    },
  },
  directives: {
    ClickOutside,
  },
};
</script>

<style scoped>
.map {
  height: 100%;
  width: 100%;
  padding: 24px;
  overflow: hidden;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}

.map-root {
  height: 100%;
  width: 100%;
  overflow: hidden;
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

::v-deep svg {
  height: 100%;
  width: 100%;
}

::v-deep .table {
  cursor: pointer;
}
</style>
