<template>
  <section class="section-post-details">
    <div class="post-title">
      <div class="container">
        <h1>{{dynamicPageItem.fields.title}}</h1>
      </div>
    </div>
    <div class="container">
      <div class="post">
        <img
          v-if="dynamicPageItem.fields.image "
          :src="dynamicPageItem.fields.image.url + '?w=860'"
          :alt="dynamicPageItem.fields.image.label"
          class="post-image"
        />
        <div class="post-content" v-html="dynamicPageItem.fields.details"></div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    contentID: Number,
    item: Object,
    page: Object,
    pageInSitemap: Object,
    dynamicPageItem: Object
  },
  mounted() {}
};
</script>

<style scoped>
.post-details {
  padding: 16px;
}
.post-details .post img {
  max-width: 100%;
}
</style>