<template>
  <v-container fluid>
    <section class="my-3 d-flex justify-end">
      <v-btn to="/"> <v-icon>mdi-arrow-left</v-icon> Go Back </v-btn>
    </section>

    <section class="post-content">
      <h2 class="text-h2 mb-10">{{ post.title }}</h2>

      <nuxt-content :document="post" />
    </section>

    <v-row>
      <v-col class="d-flex justify-space-between align-center mt-5" cols="12">
        <v-btn :disabled="!prev" :to="prev && prev.path"> Previous Post </v-btn>

        <v-btn :disabled="!next" :to="next && next.path"> Next Post </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'PostPage',
  layout: 'DefaultLayout',
  async asyncData ({ $content, error, params }) {
    const [prev, next] = await $content()
      .only(['path'])
      .sortBy('createdAt', 'desc')
      .surround(params.slug)
      .fetch()

    const post = await $content(params.slug)
      .fetch()
      .catch(() =>
        error({
          statusCode: 404,
          message: "Oops the page you are looking for does't exist."
        })
      )

    return { post, prev, next }
  }
}
</script>

<style lang="scss" scoped></style>
