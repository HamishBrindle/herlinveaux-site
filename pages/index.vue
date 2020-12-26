<template>
  <div class="page-index">
    <div class="container container--center hero">
      <span class="icon">👪</span>
      <h1 class="title">
        Herlinveaux
      </h1>
      <h3 class="subtitle">
        Under Construction - we'll have this beauty up in no time!
      </h3>
    </div>
  </div>
</template>

<script>
  import BlogSection from "~/components/Sections/BlogSection"

  import blogsEn from '~/contents/en/blogsEn.js'
  import blogsEs from '~/contents/es/blogsEs.js'

  export default {
    async asyncData ({app}) {

      const blogs = app.i18n.locale === 'en' ? blogsEn : blogsEs
      
      async function asyncImport (blogName) {
        const wholeMD = await import(`~/contents/${app.i18n.locale}/blog/${blogName}.md`)
        return wholeMD.attributes
      }

      return Promise.all(blogs.map(blog => asyncImport(blog)))
      .then((res) => {
        return {
          blogs: res
        }
      })
    },
    
    components: { BlogSection },

    transition: {
      name: 'slide-fade'
    },

    head () {
      return {
        title: this.$t('indexPageHead.title'),
        htmlAttrs: {
          lang: this.$i18n.locale,
        },
        script: [{ src: 'https://identity.netlify.com/v1/netlify-identity-widget.js' }],
        meta: [
          { name: "author", content: "Andrew Brindle" },
          { name: "description", property: "og:description", content: this.$t('indexPageHead.description'), hid: "description" },
          { property: "og:title", content: this.$t('indexPageHead.title') },
          { property: "og:image", content: this.ogImage },
          { name: "twitter:description", content: this.$t('indexPageHead.description') },
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

<style lang="scss">
.hero {
  background: #094D92;
  color: rgb(255, 248, 248);

  .icon {
    font-size: 10rem;
  }

  .title {
    font-size: 5rem;
    margin: 0 0 1rem;
  }

  .subtitle {
    opacity: 0.5;
  }
}
</style>
