<template>
  <div v-if="loading"></div>
  <v-container v-else class="d-flex justify-start">
    <board
      :key="Math.random()"
      :loading="loadingTodo"
      id="toDo"
      title="To Do"
      :cards="toDo"
      @newCard="addNewCard"
      @updateCard="updateCard"
    />
    <board
      :key="Math.random()"
      :loading="loadingDoing"
      id="doing"
      title="Doing"
      :cards="doing"
      @newCard="addNewCard"
      @updateCard="updateCard"
    />
    <board
      :key="Math.random()"
      :loading="loadingDone"
      id="done"
      title="Done"
      :cards="done"
      @newCard="addNewCard"
      @updateCard="updateCard"
    />
  </v-container>
</template>

<script>
import Board from "@/components/Board.vue";

export default {
  components: {
    Board,
  },

  data() {
    return {
      loading: true,
      loadingTodo: false,
      loadingDone: false,
      loadingDoing: false,
      cards: [],
    };
  },
  beforeMount() {
    this.getCards();
  },
  computed: {
    toDo() {
      return this.cards.filter((card) => card.board === "toDo");
    },
    doing() {
      return this.cards.filter((card) => card.board === "doing");
    },
    done() {
      return this.cards.filter((card) => card.board === "done");
    },
  },
  methods: {
    async getCards() {
      await fetch("http://127.0.0.1:3333/cards")
        .then((response) => {
          return response.json();
        })
        .then((json) => {
          this.cards = json.cards;
          this.loading = false;
        });
    },

    async addNewCard(obj) {
      this.loadBoard(obj.board, true)
      const requestOptions = {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(obj),
      };
      await fetch("http://127.0.0.1:3333/cards", requestOptions)
        .then((response) => {
          return response.json();
        })
        .then((json) => {
          this.getCards();
          this.loadBoard(obj.board, false)
        });
    },

    async updateCard(obj) {
      const requestOptions = {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(obj),
      };

      await fetch("http://127.0.0.1:3333/cards", requestOptions)
        .then((response) => {
          return response.json();
        })
        .then((json) => {
          this.getCards();
        });
    },

    loadBoard(board, state) {
      switch (board) {
        case "toDo":
          this.loadingTodo = state
          break;
        case "done":
          this.loadingDone = state
          break;
        case "Doing":
          this.loadingDoing = state
          break;
      }
    }
  },
};
</script>
