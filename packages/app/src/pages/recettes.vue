<script lang="ts" setup>
  import Cards from '../components/Cards.vue';
  import type { ITag } from '~/models/search.model'
  const { find } = useStrapi4()
  const search = useSearchStore()

  const { data: tags } = useAsyncData(
    'tags',
    () => find<{ data: ITag[] }>('tags'),
  )
  function addTag(tag: string) {
    if (!search.queryTags.includes(tag))
      search.queryTags.push(tag)
    else search.queryTags = search.queryTags.filter(t => t !== tag)
  }

</script>

<template>
  <div class="container">
    <div v-if="!search.pending && search.sortedByTags" class="flex flex-col gap-y-4">
      <Header />
      <div class="flex flex-col lg:flex-col gap-4 ">
        <div class="flex justify-around items-center gap-4">
          <div class="form-group flex flex-col gap-2 w-[60%]" role="search">
            <label for="search">Chercher par recetta :</label>
            <input id="search" v-model="search.query" class="font-[Segoe Ui] px-4 py-2 rounded-1 outline-0 border-red" type="search" placeholder="Chercher une recette...">
          </div>
          <div class="form-group flex flex-col items-start">
            <p>Filtrer par tag :</p>
            <div class="flex flex-wrap items-start gap-2" role="group">
              <button
                v-for="tag in tags?.data"
                :key="tag.id"
                :class="{ 'text-white': search.queryTags.includes(tag.slug), 'brightness-90':search.queryTags.includes(tag.slug) }"
                :style="{ backgroundColor: tag.color }"
                :title="tag.title"
                class="py-1 px-2 bg-gray-200 text-gray-900 border-none cursor-pointer"
                @click="addTag(tag.slug)"
              >
                {{ tag.title }}
              </button>
            </div>
            <button
              class="mt-4 appearance-none border-none bg-transparent p-0 underline cursor-pointer"
              @click="search.resetTags"
            >
              Réinitialiser les tags selectionnetati
            </button>
          </div>
        </div>
        <client-only>
          <div class="flex flex-col items-center p-10 px-20">
            <h2>Les recettas</h2>
            <ul
              v-if="search.sortedByTags.length"
              class="list-none p-0 sm:grid-cols-2 gap-x-4 gap-y-8 flex flex-wrap justify-around"
            >
              <Cards
                v-for="recipe in search.sortedByTags"
                :key="recipe.id"
                :recipe="recipe"
              />
            </ul>
            <p v-else>
              Aucun résultat pour cette recherche, rafraichisseti la page !
            </p>
          </div>
        </client-only>
    </div>
  </div>
  </div>
</template>