<template>
  <v-app-bar flat class="color flex justify-start">
    <v-app-bar-title class="title">
      <b class="items-color mr-3">To do List</b>
    </v-app-bar-title>
    <v-btn @click="drawer = true" size="small" variant="tonal" class="mr-2 hide-icon" color="white">
      <i class="fa-regular fa-star"></i>
    </v-btn>
    <v-divider :thickness="2" vertical class="divider" />
    <v-btn size="small" variant="tonal" class="mr-2 hide-icon" color="white">
      <i class="fa-solid fa-user-group"></i>
      Visivel à Area de Trabalho
    </v-btn>

    <v-divider :thickness="2" vertical class="divider" />
    <v-btn size="small" class="mr-2 hide-icon" variant="flat" color="#ebecf0">
      <i class="fa-solid fa-chart-simple selected-button"></i>
      <span class="selected-button">Quadro</span>
    </v-btn>
    <v-btn size="small" variant="tonal" class="mr-2" color="white">
      <i class="fa-solid fa-chevron-down"></i>
    </v-btn>
  </v-app-bar>
</template>

<script>

export default {
  data() {
    return {
      drawer: false,
      items: [1,2,3]
    };
  },
};
</script>


<style scoped>
.color {
  background-color: #694a78 !important;
}
.items-color {
  color: white;
}
.selected-button {
  color: #694a78;
}

.title {
  max-width: 100px;
}

.divider {
  color: white;
  max-height: 20px;
  align-self: center;
  margin-right: 10px;
}

@media (max-width: 600px) {
  .hide-icon {
    display: none
  }

  .divider {
    display: none;
  }
}
</style>
