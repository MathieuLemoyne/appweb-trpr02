<script setup lang="ts">
import { defineProps, defineEmits } from "vue";
import { type Game } from "../types";

const props = defineProps<{
  game: Game;
}>();

const emit = defineEmits<{
  (event: "edit", game: Game): void;
  (event: "duplicate", game: Game): void;
  (event: "remove", game: Game): void;
  (event: "show-details", game: Game): void;
}>();

const showDetails = () => {
  emit("show-details", props.game);
};

const editGame = () => {
  emit("edit", props.game);
};

const duplicateGame = () => {
  emit("duplicate", props.game);
};

const removeGame = () => {
  emit("remove", props.game);
};
</script>

<template>
  <div class="card h-100">
    <div class="card-body">
      <h2 class="card-title">{{ game.title }}</h2>
      <ul v-if="game.showDetails" class="list-unstyled">
        <li>
          <strong>Cote :</strong>
          <span v-for="n in game.rating" :key="n">⭐</span>
        </li>
        <li :class="{ 'text-danger': game.quantity === 0 }">
          <strong>Quantité :</strong> {{ game.quantity }}
        </li>
        <li><strong>Prix :</strong> {{ game.price.toFixed(2) }} $</li>
        <li><strong>Description :</strong> {{ game.description }}</li>
      </ul>
    </div>
    <div class="card-footer d-flex justify-content-between">
      <button @click="showDetails()" class="btn btn-info">🔍 Détails</button>
      <button @click="editGame()" class="btn btn-warning">✏️ Modifier</button>
      <button @click="duplicateGame()" class="btn btn-primary">
        📝 Dupliquer
      </button>
      <button @click="removeGame()" class="btn btn-danger">❌ Supprimer</button>
    </div>
  </div>
</template>
<style></style>
