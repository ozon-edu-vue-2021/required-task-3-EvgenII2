<template>
  <div class="map">
    <h3>Карта офиса</h3>

    <div v-if="!isLoading" class="map-root">
      <MapSVG ref="svg" />
      <Table v-show="false" ref="table" />
    </div>
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import Table from "@/assets/images/workPlace.svg";
import MapSVG from "@/assets/images/map.svg";
import tables from "@/assets/data/tables.json";
import legend from "@/assets/data/legend.json";
import people from "@/assets/data/people.json";
import * as d3 from "d3";

export default {
  components: {
    MapSVG,
    Table,
  },
  data() {
    return {
      isLoading: false,
      svg: null,
      g: null,
      tableSVG: null,
      tables: [],
    };
  },
  created() {
    this.tables = tables;
  },
  mounted() {
    this.isLoading = true;

    this.svg = d3.select(this.$refs.svg);
    this.svg.on("click", () => {
      this.handleClickOnMap();
    });
    this.g = this.svg.select("g");
    this.tableSVG = d3.select(this.$refs.table);
    if (this.g) {
      this.drawTables();
    } else {
      alert("SVG is incorrect");
    }
    this.isLoading = false;
  },
  methods: {
    drawTables() {
      const svgTablesGroupPlace = this.g
        .append("g")
        .classed("groupPlaces", true);
      this.tables.map((table) => {
        const onClick = (ev) => {
          this.handleClickOnTable(table._id);
          ev.stopPropagation();
        };

        const targetSeat = svgTablesGroupPlace
          .append("g")
          .attr("transform", `translate(${table.x}, ${table.y}) scale(0.5)`)
          .attr("id", table._id)
          .on("click", onClick)
          .classed("employer-place", true);

        targetSeat
          .append("g")
          .attr("transform", `rotate(${table.rotate || 0})`)
          .attr("group_id", table.group_id)
          .classed("table", true)
          .html(this.tableSVG.html())
          .attr(
            "fill",
            legend.find((it) => it.group_id === table.group_id)?.color ??
              "transparent"
          );
      });
    },
    handleClickOnTable(id) {
      this.$emit(
        "setPerson",
        people.find((person) => {
          return person.tableId === id;
        })
      );
    },
    handleClickOnMap() {
      this.$emit("setPerson", null);
    },
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
