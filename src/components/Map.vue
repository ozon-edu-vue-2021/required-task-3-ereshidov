<template>
  <div class="map">
    <h3>Карта офиса</h3>

    <div v-if="!isLoading" class="map-root">
      <MapSVG ref="svg" />
      <TableSVG v-show="false" ref="table" />
    </div>
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import * as d3 from "d3";

import tables from "../assets/data/tables.json";
import legend from '../assets/data/legend.json';

import MapSVG from "../assets/images/map.svg";
import TableSVG from "../assets/images/workPlace.svg";

export default {
  components: {
    MapSVG,
    TableSVG,
  },
  data() {
    return {
      tableSvg: null,
      isLoading: false,
      tables: null,
      svg: null,
      g: null,
    };
  },
  mounted() {
    this.svg = d3.select(this.$refs.svg);
    this.g = this.svg.select("g");
    this.tableSvg = d3.select(this.$refs.table);

    this.tables = tables;

    if (this.g) {
      this.drawTables();
    } else {
      console.error("Error happen while drawing tables");
    }
  },
  methods: {
    drawTables() {
      const svgTablesGroup = this.g.append("g").classed("groupPlaces", true);
      this.tables.map((table) => {
        const svgTable = svgTablesGroup
          .append("g")
          .attr("transform", `translate(${table.x}, ${table.y}) scale(0.5)`)
          .attr("id", table._id)
          .classed("employer-place", true);

        svgTable
          .append("g")
          .attr("transform", `rotate(${table.rotate || 0})`)
          .html(this.tableSvg.html())
          .attr("fill", this.findTableColor(table.group_id));
      });
    },
    findTableColor(tableGroupId) {
        const table = legend.find((subDivision) => subDivision.group_id === tableGroupId);

        return table?.color ?? 'transparent';
    }
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
