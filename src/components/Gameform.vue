<script setup lang="ts">
import { computed } from "vue";
import { type Game } from "../types";

const props = defineProps<{ selectedGame: Game | null }>();
const emit = defineEmits<{
  (event: "add-edit-game", game: Game): void;
}>();

const gameForm = computed(() => ({
  title: props.selectedGame?.title || "",
  rating: props.selectedGame?.rating || 0,
  quantity: props.selectedGame?.quantity || 1,
  price: props.selectedGame?.price || 0,
  description: props.selectedGame?.description || "",
  showDetails: props.selectedGame?.showDetails ?? false,
}));

const saveChanges = () => {
  if (props.selectedGame) {
    const updatedGame: Game = {
      ...props.selectedGame,
      title: gameForm.value.title,
      rating: gameForm.value.rating,
      quantity: gameForm.value.quantity,
      price: gameForm.value.price,
      description: gameForm.value.description,
      showDetails: gameForm.value.showDetails,
    };
    emit("add-edit-game", updatedGame);
  }
};

const resetForm = () => {
  gameForm.value.title = "";
  gameForm.value.rating = 0;
  gameForm.value.quantity = 1;
  gameForm.value.price = 0;
  gameForm.value.description = "";
  gameForm.value.showDetails = false;
};
</script>

<template>
  <form @submit.prevent="saveChanges">
    <div class="mb-3">
      <label for="title" class="form-label">Nom du jeu</label>
      <input
        id="title"
        v-model="gameForm.title"
        type="text"
        class="form-control"
        required
      />
    </div>

    <div class="mb-3">
      <label for="rating" class="form-label">Nombre d'étoiles</label>
      <input
        id="rating"
        v-model.number="gameForm.rating"
        type="number"
        class="form-control"
        min="0"
        max="5"
        required
      />
    </div>

    <div class="mb-3">
      <label for="quantity" class="form-label">Quantité</label>
      <input
        id="quantity"
        v-model.number="gameForm.quantity"
        type="number"
        class="form-control"
        min="0"
        required
      />
    </div>

    <div class="mb-3">
      <label for="price" class="form-label">Prix</label>
      <input
        id="price"
        v-model.number="gameForm.price"
        type="number"
        class="form-control"
        min="0.01"
        step="0.01"
        required
      />
    </div>

    <div class="mb-3">
      <label for="description" class="form-label">Description</label>
      <textarea
        id="description"
        v-model="gameForm.description"
        class="form-control"
        required
      ></textarea>
    </div>

    <button type="submit" class="btn btn-success">
      {{ selectedGame ? "Modifier" : "Ajouter" }} le jeu
    </button>
    <button
      type="button"
      class="btn btn-secondary"
      @click="resetForm"
      v-if="selectedGame"
    >
      Annuler
    </button>
  </form>
</template>
