<template>
  <section class="section-news">
    <div class="container">
      <h3>{{item.fields.title}}</h3>

      <div class="posts-list">
        <template v-for="post in posts.slice(0, item.fields.postCount)">
          <div class="post" :key="post.contentID">
            <router-link v-if="post.image != null" :to="post.to"><img :src="post.image.url" :alt="post.image.label" /></router-link>
            
            <div class="post-content">
              <h4>
                <router-link :to="post.to">{{post.title}}</router-link>
              </h4>
              <p v-html="post.excerpt"></p>
            </div>
          </div>
        </template>
      </div>
    </div>
  </section>
</template>


<script>
import truncate from "truncate-html";

export default {
  props: {
    contentID: Number,
    item: Object,
    page: Object
  },
  data: function() {
    return {
      posts: []
    };
  },
  async mounted() {
    const self = this;
    const api = self.$agility.client;
    try {
      //TODO: Need a proper way to cache the sitemap
      //get sitemap first, need it to find the dynamic urls
      let sitemap = await api.getSitemapFlat({
        channelName: self.$agility.config.channelName,
        languageCode: self.$agility.config.languageCode
      });

      //then get our posts
      let contentListResult = await api.getContentList({
        referenceName: "posts",
        languageCode: self.$agility.config.languageCode
      });

      const dynamicUrls = self.resolvePostUrls(
        sitemap,
        contentListResult.items
      );

      let posts = [];
      contentListResult.items.forEach(item => {
        let img = null;
        if (item.fields.image) {
          img = {
            url: item.fields.image.url + "?w=860",
            label: item.fields.image.label
          };
        }

        posts.push({
          contentID: item.contentID,
          to: dynamicUrls[item.contentID],
          title: item.fields.title,
          excerpt: truncate(item.fields.details, {
            stripTags: true,
            length: 160
          }),
          image: img
        });

        this.posts = posts;
      });
    } catch (error) {
      if (console) console.error(error);
    }
  },
  methods: {
    resolvePostUrls(sitemap, posts) {
      let dynamicUrls = {};
      posts.forEach(post => {
        Object.keys(sitemap).forEach(path => {
          if (sitemap[path].contentID === post.contentID) {
            dynamicUrls[post.contentID] = path;
          }
        });
      });
      return dynamicUrls;
    }
  }
};
</script>


<style scoped>
.posts-listing .post {
  padding: 16px;
  margin-bottom: 20px;
  padding-bottom: 20px;
  border-bottom: 1px solid #dfdfdf;
}

.posts-listing .post img {
  max-width: 200px;
  margin-right: 20px;
}
</style>