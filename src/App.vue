<template>
  <div id="app">
    <div class="office">
      <Map :person.sync="person" :isUserOpened.sync="isUserOpened" :legend="legend" :tables="tables" />
      <SideMenu :person="person" :isUserOpenned="isUserOpened" :legend.sync="legend" />
    </div>
  </div>
</template>

<script>
import Map from "./components/Map.vue";
import SideMenu from "./components/SideMenu.vue";
import tables from "./assets/data/tables.json";

export default {
  name: "App",
  components: {
    Map,
    SideMenu,
  },
  data() {
    return {
      tables: [],
      legend: [],
      isUserOpened: false,
      person: null,
    };
  },
  created() {
    this.tables = tables;
    this.generateLegend();
  },
  methods: {
    generateLegend() {
      const legend = {};
      this.tables.forEach((table) => {
        const currentTableGroupId = table.group_id;
        if (legend[currentTableGroupId]) {
          legend[currentTableGroupId].counter += 1;
        } else {
          legend[currentTableGroupId] = {
            text: `Подразделение ${Object.keys(legend).length + 1}`,
            group_id: currentTableGroupId,
            color: `#${Math.floor(Math.random() * 16777215).toString(16)}`,
            counter: 1,
          };
        }
      });

      this.legend = Object.values(legend);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  background-color: #fafafa;
  padding: 24px;
  box-sizing: border-box;
}

html,
body,
#app {
  height: 100%;
}

* {
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

.office {
  display: grid;
  grid-template-columns: 1fr 320px;
  border-radius: 6px;
  border: 1px solid #ccd8e4;
  height: 100%;
  background: white;
  max-width: 1500px;
  margin: 0 auto;
}
</style>
