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
      <h2 class="w-fit m-auto">Mama mia, régales-toi avec les migliori ritzete de la mama</h2>
  </div>
  </div>
</template>