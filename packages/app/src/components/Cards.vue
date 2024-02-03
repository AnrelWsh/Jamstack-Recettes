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
  <li class="w-[300px] shadow-xl rounded-md hover:shadow-2xl hover:translate-x-[10px] hover:translate-y-[-10px] transition"> 
    <NuxtLink :to="`/recipes/${recipe.slug}`" class="no-underline">
      <div class="flex flex-col text-black">
        <NuxtImg :src="recipe.image.url" alt="recipe image" class="font-[Segoe Ui] w-[100%] h-[150px] object-cover"/>
        <div class="p-4">
          <h2>{{ recipe.title }}</h2>
          <div class="flex items-center gap-x-2">
            <p v-for="tag in recipe.tags" :key="tag.id" class="py-1 px-2"
            :style="{ backgroundColor: tag.color }">
              {{ tag.title }}
            </p>
          </div>
        </div>
      </div>
    </NuxtLink>
  </li>
</template>