<template lang="pug">
  .postslist
    ul
      li.show-dark-arrow-on-hover(v-for='(page, index) in pages', :key='index')
        span.d-inline-block
          router-link.postlink(:to='page.path')
            h2
              | {{ page.frontmatter.title }} 
              small(v-if='page.frontmatter.publishdate') {{page.frontmatter.publishdate}}
              small(v-if='page.date')
                span.shh - 
                | {{ page.date }}
              |  &nbsp;
              img(src='/assets/arrow-right.svg', class='inline-arrow')
              img(src='/assets/arrow-right.dark.svg', class='inline-arrow-dark')
</template>

<script>
import dateFormat from 'dateformat'

export default {
  props: ['max'],
  components: { },
  computed: {
    data () {
      return this.$page.frontmatter
    },
    pages () {
      return this.$site.pages.filter(page => 
        page.path.indexOf("/posts/") === 0 && page.path !== '/posts/'
      ).slice(0, this.max != null ? parseInt(this.max) : Math.Infinity).map(val => {
        if (val.frontmatter.date == null) return val
        val.date = dateFormat(new Date(val.frontmatter.date), `mmmm d, yyyy`)
        return val
      })
    }
  },
  methods: {
    reachedMax (index) {
      if (this.max == null || this.max == -1) return true
      return index < this.max
    }
  }
}
</script>
