<script setup lang="ts">
const prismic = usePrismic()
const { data: articles } = useAsyncData('$articles', () =>
  prismic.client.getAllByType("article", {
    orderings: [
      { field: "my.article.publishDate", direction: "desc" },
      { field: "document.first_publication_date", direction: "desc" },
    ],
  })
)
const settings = useSettings()

const {
  withHeaderProfile,
  withHeaderDivider,
  withFooterSignUpForm
} = useLayout()
onMounted(() => {
  withHeaderProfile.value = true
  withHeaderDivider.value = false
  withFooterSignUpForm.value = true
})

useHead({
  title: computed(() => prismic.asText(settings.value?.data.name))
})
</script>

<template>
  <Bounded size="widest">
    <ul class="grid grid-cols-1 gap-16">
      <ArticleListItem
        v-for="article in articles"
        :key="article.id"
        :article="article"
      />
    </ul>
  </Bounded>
</template>
