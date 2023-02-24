<template>
  <v-card :loading="loading" class="card py-2 pl-2 mr-2">
    <div class="d-flex justify-space-between">
      <h4 class="color">{{ title }}</h4>
      <i class="fa-solid fa-ellipsis color mr-4"></i>
    </div>

    <draggable
      v-model="myArray"
      :id="id"
      group="people"
      @start="drag = true"
      @end="updateCard($event)"
      item-key="id"
    >
      <template #item="{ element }">
        <v-card class="ml-1 mr-3 mb-1 px-2 py-1" :id="element.id">
          {{ element.title }}
        </v-card>
      </template>
    </draggable>
    <v-text-field
      v-model="nameCard"
      v-if="createCard"
      placeholder="Criando um card..."
      variant="solo"
      class="mr-3 ml-1"
      @keyup.enter="addNewCard()"
    />
    <v-btn
      v-if="!createCard"
      size="small"
      variant="plain"
      class="mr-2 d-flex justify-space-between tamanho"
      @click="openNewCard()"
    >
      <i class="fa-solid fa-plus mr-2 plus-icon"></i>
      <span class="span-add">Adicionar</span>
      <span class="span-a-card"> um cartão</span>
      <i class="fa-solid fa-photo-film ml-2 append-icon hide-850"></i>
    </v-btn>
    <div v-else class="d-flex justify-space-between">
      <div>
        <v-btn
          size="small"
          variant="flat"
          color="primary"
          @click="addNewCard()"
        >
          <span>Adicionar</span>
          <span class="span-a-card ml-1">cartão</span>
        </v-btn>
        <v-btn size="small" variant="plain" class="hide-850">
          <i class="fa-solid fa-xmark big-icon align-self-center color"></i>
        </v-btn>
      </div>
      <i class="fa-solid fa-ellipsis color big-icon mr-4 hide-850"></i>
    </div>
  </v-card>
</template>

<script>
import draggable from "vuedraggable";

export default {
  components: {
    draggable,
  },
  props: {
    title: { type: String, default: "" },
    cards: { type: Array, default: [] },
    id: { type: String, default: "" },
    loading: { type: Boolean, default: false },
  },

  data() {
    return {
      nameCard: "",
      createCard: false,
      myArray: this.cards,
      drag: false,
      small: false
    };
  },
  methods: {
    openNewCard() {
      this.createCard = true;
    },
    addNewCard() {
      let board = "";
      const title = this.nameCard;

      switch (this.title) {
        case "To Do":
          board = "toDo";
          break;
        case "Done":
          board = "done";
          break;
        case "Doing":
          board = "doing";
          break;
      }

      const obj = {
        title,
        board,
      }

      this.$emit('newCard', obj)
      this.createCard = false;
      this.nameCard = "";
    },
    updateCard(event) {
      this.drag = false
      
      const obj = {
        id: event.item.id,
        board: event.to.id,
      }

      this.$emit('updateCard', obj)
    }
  },
};
</script>

<style scoped>
.card {
  min-width: 27vw;
  background-color: #ebecf0;
  height: fit-content;
}

.color {
  color: #535655;
}

.tamanho {
  width: 100%;
}

.append-icon {
  position: absolute;
  right: 10px;
}

.big-icon {
  font-size: 24px;
}

@media (max-width: 850px) {
  .hide-850 {
    display: none
  }
}

@media (max-width: 700px) {
  .span-a-card {
    display: none
  }
}
@media (max-width: 450px) {
  .span-add {
    display: none
  }

  .plus-icon {
    justify-content: center;
  }
}
</style>
