<template>
  <div>
    <h1>Author: {{ articles[0].author.name }}</h1>
    <p>Bio: {{ articles[0].author.bio }}</p>
    <h3>Here are a list of articles by {{ articles[0].author.name }}:</h3>
    <ul>
      <li v-for="article in articles" :key="article.slug">
        <NuxtLink :to="{ name: 'blog-slug', params: { slug: article.slug } }">
          <img :src="article.img" :alt="article.alt" />
          <div>
            <h2>{{ article.title }}</h2>
            <p>{{ article.description }}</p>
            <p>{{ formatDate(article.updatedAt) }}</p>
          </div>
        </NuxtLink>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'PageBlogAuthor',

  async asyncData({ $content, params }) {
    const articles = await $content('articles', params.slug)
      .where({
        // before this add `nestedProperties: ['author.name']` to content section in nuxt.config.js
        'author.name': {
          $regex: [params.author, 'i'], // case insensitive
        },
      })
      .without(['body', 'toc', 'extension'])
      .sortBy('createdAt', 'asc')
      .fetch()

    return {
      articles,
    }
  },

  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('en', options)
    },
  },
}
</script>
