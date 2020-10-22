<template>
  <article>
    <h1>{{ article.title }}</h1>
    <p class="mb-2">{{ article.description }}</p>
    <img :src="article.image" :alt="article.alt" class="mb-2" />
    <p>Article last updated: {{ formatDate(article.updatedAt) }}</p>

    <h2>Table of contents</h2>
    <nav>
      <ul class="ml-2 mb-2">
        <li v-for="link of article.toc" :key="link.id">
          <NuxtLink
            :to="`#${link.id}`"
            :class="{ 'py-2': link.depth === 2, 'ml-2 pb-2': link.depth === 3 }"
          >
            &middot; {{ link.text }}
          </NuxtLink>
        </li>
      </ul>
    </nav>

    <nuxt-content :document="article" />

    <!-- начиная с Nuxt v2.13 компоненты в папке components импортируются автоматически, достаточно написать в Nuxt конфиге: components: true -->
    <author :author="article.author" />
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
