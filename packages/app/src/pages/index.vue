<script lang="ts" setup>
  import Cards from '../components/Cards.vue';
  import type { ITag } from '~/models/search.model'
  const { find } = useStrapi4()
  const search = useSearchStore()

  const { data: tags } = useAsyncData(
    'tags',
    () => find<{ data: ITag[] }>('tags'),
  )


  // Define a computed property to limit the number of recipes shown
  const limitedRecipes = computed(() => {
    const limit = 3; // Set your desired limit here
    return search.sortedByTags.slice(0, limit);
  });
</script>

<template>
  <Header />
  <div class="container">
    <div v-if="!search.pending && search.sortedByTags" class="flex flex-col gap-y-4">
      <h2 class="m-auto mt-4 w-[80%] text-center">Mama mia, régales-toi avec les migliori ritzete de la mama</h2>
      <client-only>
        <div class="flex flex-col items-center p-10 px-20">
          <h2>Les recettas</h2>
          <ul
            v-if="limitedRecipes.length"
            class="list-none p-0 sm:grid-cols-2 gap-x-4 gap-y-8 flex flex-wrap justify-around"
          >
            <Cards
              v-for="recipe in limitedRecipes"
              :key="recipe.id"
              :recipe="recipe"
            />
          </ul>
          <p v-else>
            Aucun résultat pour cette recherche, rafraichisseti la page !
          </p>
          <NuxtLink :to="`/recettes`" :key="refreshKey" class="no-underline text-blue hover:underline transition px-4">Voir toutes les recettes</NuxtLink>
        </div>
      </client-only>
  </div>
  </div>
</template>
