<script lang="ts" setup>
  import Header from '../components/Header.vue';
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
  </div>
  </div>
</template>