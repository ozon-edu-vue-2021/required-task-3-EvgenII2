<template>
  <div id="app">
    <div class="office">
      <Map v-on:setPerson="setPersonToSideMenu" />
      <SideMenu
        v-bind:person="person"
        v-bind:isUserOpenned="isUserOpenned"
        v-on:update="closePersonCard"
      />
    </div>
  </div>
</template>

<script>
import Map from "./components/Map.vue";
import SideMenu from "./components/SideMenu.vue";

export default {
  name: "App",
  data: () => {
    return { person: null, isUserOpenned: false };
  },
  components: {
    Map,
    SideMenu,
  },
  methods: {
    setPersonToSideMenu(person) {
      this.person = person;
      if (this.person) {
        this.isUserOpenned = true;
      } else {
        this.isUserOpenned = false;
      }
    },
    closePersonCard(isUserOpenned) {
      this.person = null;
      this.isUserOpenned = isUserOpenned;
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
