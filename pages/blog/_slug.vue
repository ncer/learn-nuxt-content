<template>
  <article>
    <h1>{{ article.title }}</h1>
    <p class="mb-2">{{ article.description }}</p>
    <img :src="article.image" :alt="article.alt" class="mb-2" />
    <p>Article last updated: {{ formatDate(article.updatedAt) }}</p>

    <div v-if="article.toc.length" class="my-4">
      <h2>Table of contents</h2>
      <nav>
        <ul class="ml-2">
          <li v-for="link of article.toc" :key="link.id">
            <NuxtLink
              :to="`#${link.id}`"
              :class="{
                'py-2': link.depth === 2,
                'ml-2 pb-2': link.depth === 3,
              }"
            >
              &middot; {{ link.text }}
            </NuxtLink>
          </li>
        </ul>
      </nav>
    </div>

    <nuxt-content :document="article" />

    <NuxtLink :to="`/blog/author/${article.author.name}`">
      <author :author="article.author" class="my-4" />
    </NuxtLink>

    <hr />

    <!-- начиная с Nuxt v2.13 компоненты в папке components импортируются автоматически, достаточно написать в Nuxt конфиге: components: true -->
    <prev-next :prev="prev" :next="next" class="my-4" />
  </article>
</template>

<script>
export default {
  name: 'PageBlogPost',

  async asyncData({ $content, params }) {
    const article = await $content('articles', params.slug).fetch()

    const [prev, next] = await $content('articles')
      .only(['title', 'slug'])
      .sortBy('createdAt', 'asc')
      .surround(params.slug) // here all the magic
      .fetch()

    return {
      article,
      prev,
      next,
    }
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
.nuxt-content h2 {
  font-weight: bold;
  font-size: 24px;
}
.nuxt-content h3 {
  font-weight: bold;
  font-size: 20px;
}
.nuxt-content p {
  margin-bottom: 20px;
}

.nuxt-content-highlight {
  @apply relative;
}
.nuxt-content-highlight .filename {
  @apply absolute right-0 text-gray-400 z-10 mr-2 mt-1 text-sm font-mono;
}
</style>
