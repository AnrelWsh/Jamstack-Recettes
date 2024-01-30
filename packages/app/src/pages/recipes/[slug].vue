<script lang="ts" setup>
  const {findOne} = useStrapi4()
  const route = useRoute()

  const { data: recipe, pending, error, refresh } = useAsyncData(
    'recipe',
    () => findOne(`recipes/${route.params.slug}`, { 
  })
  )
</script>

<template>
    <template v-if="pending">Loading ...</template>
    <template v-if="recipe">
        <h1>{{ recipe.data.title }}</h1>
        <p>{{ recipe.data.description }}</p>
    </template>
</template>