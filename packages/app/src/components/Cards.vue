<script lang="ts" setup>
  import type { IRecipe } from '../models/search.model.ts'

  defineProps({
    recipe: {
      type: Object as PropType<IRecipe>,
      required: true,
    },
  })
</script>

<template>
          <li>
            <div class="flex flex-col">
              <h2>{{ recipe.title }}</h2>
              <div class="flex items-center gap-x-2">
                <p v-for="tag in recipe.tags" :key="tag.id" class="py-1 px-2 bg-gray-200">
                  {{ tag.title }}
                </p>
              </div>
              <p class="my-0 truncate"></p>
              <NuxtImg :src="recipe.image.url" alt="recipe image" />
              <NuxtLink :to="`/recipes/${recipe.slug}`">
              Voir recette
              </NuxtLink>
            </div>
          </li>
</template>