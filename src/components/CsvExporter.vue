<script setup lang="ts">
import { defineProps } from "vue";
import { type Game } from "../types";

const props = defineProps<{ games: Game[] }>();

const exportToCSV = () => {
  const headers = ["Titre", "Cote", "Quantité", "Prix", "Description"];
  const rows = props.games.map((game) => [
    game.title,
    game.rating,
    game.quantity,
    game.price.toFixed(2),
    game.description,
  ]);

  const csvContent = [headers, ...rows]
    .map((row) => row.map((value) => `"${value}"`).join(","))
    .join("\n");

  const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" });
  const link = document.createElement("a");
  link.href = URL.createObjectURL(blob);
  link.download = "wishlist.csv";
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
};
</script>

<template>
  <button @click="exportToCSV" class="btn btn-success">📂 Exporter CSV</button>
</template>
