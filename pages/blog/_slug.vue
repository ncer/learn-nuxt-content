<template>
  <article>
    <h1>{{ article.title }}</h1>
    <p class="mb-2">{{ article.description }}</p>
    <img :src="article.image" :alt="article.alt" class="mb-2" />

    <nuxt-content :document="article" />

    <p>Article last updated: {{ formatDate(article.updatedAt) }}</p>
  </article>
</template>

<script>
export default {
  name: 'SlugVue',

  async asyncData({ $content, params }) {
    const article = await $content('articles', params.slug).fetch()

    return { article }
  },

  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('ru', options)
    },
  },
}
</script>

<style>
img {
  max-width: 400px;
}
h1 {
  font-weight: bold;
  font-size: 32px;
}
h2 {
  font-weight: bold;
  font-size: 28px;
}
.nuxt-content-container h2 {
  font-weight: bold;
  font-size: 24px;
}
.nuxt-content-container h3 {
  font-weight: bold;
  font-size: 20px;
}
.nuxt-content-container p {
  margin-bottom: 20px;
}
</style>
