<template>
  <div class="page-index">
    <div class="container">
      <BlogSection :blogs="blogs"/>
    </div>
  </div>
</template>

<script>
  import BlogSection from "~/components/Sections/BlogSection"

  export default {
    async asyncData ({app}) {
    },
    
    components: { BlogSection },

    head () {
      return {
        title: "Workshop Nuxt blog",
        htmlAttrs: {
          lang: this.$i18n.locale,
        },
        meta: [
          { name: "author", content: "Marina Aisa" },
          { name: "description", property: "og:description", content: "Workshop Nuxt blog", hid: "description" },
          { property: "og:title", content: "Workshop Nuxt blog" },
          { property: "og:image", content: this.ogImage },
          { name: "twitter:description", content: "Workshop Nuxt blog" },
          { name: "twitter:image", content: this.ogImage }
        ]
      };
    },

    computed: {
      ogImage: function () {
        return;
      }
    }
  }
</script>
