<script setup lang="ts">
import { type Game } from "../types";

defineProps<{ game: Game }>();
</script>

<template>
  <span>{{ game.title }}</span>
</template>
