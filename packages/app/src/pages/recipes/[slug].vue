<script lang="ts" setup>
  import MarkdownIt from 'markdown-it';

  const { findOne } = useStrapi4();
  const route = useRoute();
  const md = new MarkdownIt();

  const { data: recipe, pending, error, refresh } = useAsyncData(
    'recipe',
    () => findOne(`recipes/${route.params.slug}`, {})
  );

  const renderMarkdown = (text: string) => {
    return md.render(text);
  };
</script>

<template>
  <Header />
  <template v-if="pending">Loading ...</template>
  <template v-if="recipe" class="w-[100%]">
    <div class="relative">
      <NuxtImg :src="recipe.data.image.url" alt="recipe image" class="w-[100%] h-[300px] object-cover"/>
      <h1 class="absolute top-[40%] left-10">{{ recipe.data.title }}</h1>
    </div>
    
    <NuxtLink :to="`/recettes`"  :key="refreshKey"  class="no-underline text-blue hover:underline transition px-4">Retour aux recettes</NuxtLink>
    <div class="w-[100%] py-8">
      <h2 class="w-fit m-auto">Ritzeta :</h2>
      <div class="w-[80%] m-auto" v-html="renderMarkdown(recipe.data.description)"></div>
    </div>
  </template>
</template>