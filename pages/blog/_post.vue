<template>
  <div v-editable="story.content" :style="cssVars">
    <article class="post">
      <div class="post__header">
        <div class="container">
          <div class="post__title">
            <h3>
              <nuxt-link :key="category.id" v-for="category in categories" :to="'/' + category.full_slug">
                <span>{{ category.content.name }}</span>
              </nuxt-link>
            </h3>
            <h1>{{ post.content.title }}</h1>
            <p>{{ post.content.intro }}</p>
            <div class="meta">
              <figure v-if="author">
                <nuxt-link :to="'/' + author.full_slug">
                  <img v-bind:class="[$route.fullPath == '/blog/panama-city' ? 'greyscale' : '']" class="author" :src="resize(author.content.avatar, '140x140')" :alt="author.content.name">
                </nuxt-link>
              </figure>
              <div>
                <h2 v-if="author">Geschrieben von <nuxt-link :to="'/' + author.full_slug">{{ author.content.name }}</nuxt-link></h2>
                <p>{{ formatDate(post.first_published_at, 'DD. MMM YYYY') }} • {{ readTime(post.content.body) }} Min. Lesezeit</p>
              </div>
            </div>
          </div>
          <figure class="post__image">
            <picture>
              <source
                media="(min-width: 768px)"
                :srcset="resize(post.content.image, '860x0/filters:format(webp)')"
                type="image/webp">
              <source
                media="(min-width: 768px)"
                :srcset="resize(post.content.image, '860x0')">
              <img
                :src="resize(post.content.image, '750x0')"
                :alt="post.content.title">
            </picture>
          </figure>
        </div>
      </div>

      <div class="container">
        <div class="post__body">
          <div v-html="body"></div>
          <article-tool :title="post.content.title"/>
        </div>
      </div>
    </article>

    <section class="related" v-if="related.length">
      <div class="container">
        <h2>Ähnliche Beiträge</h2>
      </div>
      <skid :stories="related"/>
    </section>

    <progress-bar/>

  </div>
</template>

<script>
import Vue from 'vue'
import marked from 'marked'
import storyblokLivePreview from '@/mixins/storyblokLivePreview'
import { markdown, resize, formatDate, readTime } from '@/plugins/helper'
import ProgressBar from '~/components/ProgressBar.vue'
import ArticleTool from '~/components/ArticleTool.vue'
import mediumZoom from 'medium-zoom'

const initAfterMount = () => {
  mediumZoom('.post img:not(.author):not(.medium-zoom-image)')
}

export default {
  head () {
    return {
      title: this.post.content.title + ' - Solmates',
      meta: [
        { hid: 'description', name: 'description', content: this.post.content.intro },
        { hid: 'og:type', propery: 'og:type', content: 'article' },
        { hid: 'og:title', propery: 'og:title', content: this.post.content.title },
        { hid: 'og:description', propery: 'og:description', content: this.post.content.intro },
        { hid: 'og:image', propery: 'og:image', content: 'https:' + resize(this.post.content.image, '1200x630') },
        { hid: 'og:url', property: 'og:url', content: 'https://www.solmates.at' + this.$route.fullPath },
        { hid: 'twitter:title', name: 'twitter:title', content: this.post.content.title },
        { hid: 'twitter:description', name: 'twitter:description', content: this.post.content.intro },
        { hid: 'twitter:image', name: 'twitter:image', content: 'https:' + resize(this.post.content.image, '1200x630') }
      ],
      __dangerouslyDisableSanitizers: ['script'],
      script: [{ innerHTML: JSON.stringify({
        "@context": "http://schema.org",
        "@type": "BlogPosting",
        "mainEntityOfPage": "https://www.solmates.at" + this.$route.fullPath,
        "headline": this.post.content.title,
        "datePublished": this.post.first_published_at,
        "dateModified": this.post.published_at,
        "description": this.post.content.intro,
        "author": {
            "@type": "Person",
            "name": this.author ? this.author.content.name : "Solmates"
        },
        "publisher": {
            "@type": "Organization",
            "name": "Solmates",
            "logo": {
                "@type": "ImageObject",
                "url": "https://www.solmates.at/solmates-logo.png",
                "width": "200",
                "height": "200"
            }
        },
        "image": {
            "@type": "ImageObject",
            "url": "https:" + resize(this.post.content.image, '1200x630'),
            "width": "1200",
            "height": "630"
        }
      }), type: 'application/ld+json' }],
      link: [
        { rel: 'canonical', href: 'https://www.solmates.at' + this.$route.fullPath },
      ],
      bodyAttrs: {
        class: 'page-blog',
        style: this.post.content.textcolor == null || this.post.content.textcolor == "" ? '' : '--site-header-tint: ' + this.post.content.textcolor
      }
    }
  },
  data () {
    return {
      story: {
        content: { body: '' }
      }
    }
  },
  components: {
    ProgressBar,
    ArticleTool
  },
  computed: {
    body () {
      return markdown(this.post.content.body, '860x0')
    },
    cssVars() {
      return {
        '--background': this.post.content.background,
        '--text-color': this.post.content.textcolor,
        '--deco-color': this.post.content.decocolor
      }
    }
  },
  mounted: function() {
    // this will be called when any component is mounted!
    initAfterMount()
  },
  mixins: [storyblokLivePreview],
  methods: {
    resize,
    formatDate,
    readTime
  },
  async asyncData (context) {
    let version = context.query._storyblok || context.isDev ? 'draft' : 'published'

    const { data } = await context.app.$storyapi.get(`cdn/stories${context.route.path}`, {
      version: version,
      cv: context.store.state.cacheVersion,
      resolve_relations: "author,categories,related_posts"
    })

    return {
      post: data.story,
      categories: data.story.content.categories,
      author: data.story.content.author,
      related: data.story.content.related_posts
    };
  }
}
</script>

<style lang="scss">
.post {
  .post__image {
    position: relative;

    @include breakpoint(m) {
      flex-basis: 50%;
      margin: 0 0 50px;
    }

    @media print {
      position: relative;
      max-width: 450px;
      margin: 0 auto;
    }

    img {
      margin: 0 auto;
    }
  }

  .post__header {
    background: var(--background);
    color: var(--text-color);
    margin-bottom: 30px;
    padding-top: 90px;
    position: relative;

    @include breakpoint(767, max) {
      &::before {
        background-color: #fff;
        bottom: 0;
        content: '';
        height: 40px;
        position: absolute;
        width: 100%;
      }
    }

    @media print {
      min-height: 0;
      padding-top: 20px;

      &::before {
        display: none;
      }
    }

    > .container {
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      margin: 30px auto 0;

      @include breakpoint(m) {
          margin: 0 auto;
      }
    }

    @include breakpoint(m) {
      min-height: 375px;
      margin-bottom: 50px;
    }

    @include breakpoint(l) {
      padding-top: 150px;
    }

    @include breakpoint(xl) {
      min-height: 626px;
    }

    @media print {
      background: none;
      color: $tint;
    }

    .post__title {
      @include breakpoint(m) {
        flex-basis: 50%;
        padding: 0 30px;
        max-width: 510px;
      }

      // categories
      h3 {
        font-family: $fs-sans;
        font-weight: 500;
        font-size: 1.5rem;
        text-transform: uppercase;
        margin: 0 0 10px;

        @include breakpoint(m) {
          font-size: 1.6rem;
        }

        a {
          color: var(--deco-color, $cta);

          @media print {
            color: $tint;
          }

          &:hover span {
            text-decoration: underline;
          }

          &::after {
            content: ', ';
          }

          &:last-child::after {
            display: none;
          }
        }
      }

      h1 {
        margin: 0 0 15px;
      }

      > p {
        color: var(--text-color);
        margin-bottom: 30px;
        font-size: 1.8rem;
        line-height: 1.4;

        @include breakpoint(l) {
          font-size: 2rem;
        }

        @media print {
          color: $tint;
        }
      }
    }
  }

  .meta {
    margin: 40px 0;
    min-height: 70px;

    @include breakpoint(m) {
      margin-bottom: 50px;
    }

    @media print {
      margin: 20px 0;
    }

    a:hover {
      text-decoration: underline;
    }

    * {
      color: var(--text-color);

      @media print {
        color: $tint;
      }
    }
  }

  .author {
    &.greyscale {
      filter: gray;
      filter: grayscale(1);
    }
  }

  .post__body {
    max-width: 700px;
    margin: 0 auto 80px;
    position: relative;

    p, ul, ol, table {
      font-family: $fs-serif;
      font-size: 2rem; //1.7rem;
      line-height: 1.55; //1.7;
      margin-bottom: 20px;

      @include breakpoint(l) {
        font-size: 2.2rem; //1.8rem;
        margin-bottom: 30px;
      }

      @media print {
        font-size: 1.8rem;
      }

      em {
        font-style: italic;
      }
    }

    li {
      a {
        text-decoration: underline;
        transition: all .1s ease-out;

        &:hover {
          text-decoration: none;
        }
      }
    }

    &::after {
      content: '';
      @include squiggly;

      @media print {
        display: none;
      }
    }

    > div {
      position: relative;
      z-index: 2;

      > p:first-of-type {
        &::first-letter {
          color: $tint;
          font-family: $fs-serif;
          font-size: 6.5rem; //7.2rem;
          font-weight: 400;
          line-height: 1;
          float: left;
          margin: 1px 10px -7px 0;

          @include breakpoint(l) {
            font-size: 6.7rem; //7.6rem;
          }
        }
      }
    }

    img {
      display: block;
      margin: 0 auto;

      @media print {
        position: relative;
        max-width: 450px;
      }

      + em {
        display: inherit;
        line-height: 1.2;
        margin-top: 12px;
        text-align: center;

        + img {
          margin-top: 20px;

          @include breakpoint(l) {
            margin-top: 30px;
          }
        }
      }

      + img {
        margin-top: 20px;

        @include breakpoint(l) {
          margin-top: 30px;
        }
      }

      &[src$="#card"] {
        border-radius: 6px;
        box-shadow: 0 4px 12px 0 rgba(0,0,0,.16);
      }

      @include breakpoint(l) {
        margin: 0 -80px;
        max-width: calc(100% + 160px);

        &[src$="gif"], &[src$="#card"] {
          margin: 0 auto;
          max-width: 100%;
        }
      }
    }

    .columns {
      > p > em {
        display: inherit;
        flex-basis: 100%;
        justify-content: center;
        text-align: center;
        margin-top: -10px;
      }

      p {
        align-items: flex-start;
        display: flex;

        img {
          margin: 0;
          max-width: calc(50% - 8px);

          &:first-child {
            margin-right: 8px;
          }

          &:last-child {
            margin-left: 8px;
          }
        }
      }

      @include breakpoint(l) {
        p {
          margin-left: -80px;
          margin-right: -80px;
          width: calc(100% + 160px);

          img {
            max-width: calc(50% - 15px);

            &:first-child {
              margin-right: 15px;
            }

            &:last-child {
              margin-left: 15px;
            }
          }
        }
      }

      @media print {
        position: relative;
        page-break-inside: avoid;
        max-width: 450px;
        margin: 0 auto;

        p {
          position: relative;
          page-break-inside: avoid;
        }
      }
    }

    .wrapper {
      width: 100%;
      margin-bottom: 20px;

      @include breakpoint(l) {
        margin-bottom: 30px;
      }

      @media print {
        display: none;
      }

      > div {
        position: relative;
        width: 100%;
        height: 0;
        padding-bottom: 56.25%;
      }

      > p > em {
        display: inherit;
        margin-top: 20px;
        text-align: center;
      }

      @include breakpoint(l) {
        margin-left: -80px;
        margin-right: -80px;
        width: calc(100% + 160px);
      }

      iframe {
         position:absolute;
         left: 0;
         top: 0;
         height: 100%;
         width: 100%;
      }
    }
  }
}

.related {
  margin-bottom: 80px;
  position: relative;

  h2 {
    margin: 0 0 30px;
    text-align: center;
  }

  @media print {
    display: none;
  }
}
</style>
