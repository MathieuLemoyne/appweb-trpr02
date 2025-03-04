<script setup lang="ts">
import { ref } from "vue";
import GameCard from "./Gamecard.vue";
import GameForm from "./Gameform.vue";
import { type Game } from "../types";
import CsvExporter from "./CsvExporter.vue";

const wishlist = ref<Game[]>([
  {
    title: "The Legend of Zelda: Breath of the Wild",
    rating: 3,
    quantity: 5,
    price: 59.99,
    description: "Un jeu d'aventure épique dans un monde ouvert.",
    showDetails: false,
  },
  {
    title: "Elden Ring",
    rating: 4,
    quantity: 12,
    price: 69.99,
    description: "Un action-RPG développé par FromSoftware.",
    showDetails: false,
  },
  {
    title: "Cyberpunk 2077",
    rating: 4,
    quantity: 4,
    price: 49.99,
    description: "Un RPG futuriste en monde ouvert.",
    showDetails: false,
  },
  {
    title: "Hades",
    rating: 3,
    quantity: 8,
    price: 24.99,
    description:
      "Un roguelike intense avec des combats rapides et une narration riche.",
    showDetails: false,
  },
  {
    title: "Hollow Knight",
    rating: 5,
    quantity: 6,
    price: 14.99,
    description:
      "Un jeu de plateforme-action dans un monde souterrain mystérieux.",
    showDetails: false,
  },
  {
    title: "Red Dead Redemption 2",
    rating: 5,
    quantity: 3,
    price: 59.99,
    description: "Un western en monde ouvert avec une histoire immersive.",
    showDetails: false,
  },
  {
    title: "The Witcher 3: Wild Hunt",
    rating: 4,
    quantity: 10,
    price: 39.99,
    description:
      "Un RPG d'action avec un monde ouvert riche et des quêtes captivantes.",
    showDetails: false,
  },
  {
    title: "Stardew Valley",
    rating: 5,
    quantity: 15,
    price: 14.99,
    description: "Un simulateur de ferme relaxant avec des éléments de RPG.",
    showDetails: false,
  },
]);
const selectedGame = ref<Game | null>(null);

const removeGame = (game: Game) => {
  wishlist.value = wishlist.value.filter((g) => g !== game);
};

const resetForm = () => {
  selectedGame.value = null;
};

const addOrEditGame = (updatedGame: Game) => {
  if (selectedGame.value) {
    Object.assign(selectedGame.value, updatedGame);
  } else {
    wishlist.value.push({ ...updatedGame, showDetails: false });
  }
  resetForm();
};

const editGame = (game: Game) => {
  selectedGame.value = game;
};

const duplicateGame = (game: Game) => {
  wishlist.value.push({ ...game, showDetails: false });
};

const showDetails = (game: Game) => {
  game.showDetails = !game.showDetails;
};
</script>

<template>
  <h1>Marketplace sans vapeur</h1>
  <GameForm
    :selectedGame="selectedGame"
    @add-edit-game="addOrEditGame"
    @reset-form="resetForm"
  />
  <CsvExporter :games="wishlist" />

  <div class="row mt-4">
    <div class="col-md-6 mb-3" v-for="game in wishlist" :key="game.title">
      <GameCard
        :game="game"
        @edit="editGame"
        @duplicate="duplicateGame"
        @remove="removeGame"
        @show-details="showDetails"
      />
    </div>
  </div>
</template>
