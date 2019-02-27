<template lang="pug">
  article.post(:style="{ 'margin-bottom': bottompad === false ? '0 !important' : null }")
    h1(v-if='showtitle !== false')
      //- small(v-if='$page.frontmatter.author != null')
      //-   | {{ $page.frontmatter.author }}
      //-   span.shh :&nbsp;
      | {{ $page.title }} 
      small(v-if='lastUpdated')
        span.shh - 
        | {{ lastUpdated }}
    Content(:custom='true')
    p.faded(v-if='$page.frontmatter.author != null')
      i — {{ $page.frontmatter.author }}
</template>

<script>
import { resolvePage, normalize, outboundRE, endingSlashRE } from './util'
import dateFormat from 'dateformat'

export default {
  props: ['showtitle', 'bottompad'],
  computed: {
    lastUpdated () {
      if (this.$page.frontmatter.date == null) return null
      const date = new Date(this.$page.frontmatter.date)
      return dateFormat(date, `mmmm d, yyyy`)
    },
    prev () {
      const prev = this.$page.frontmatter.prev
      if (prev === false) {
        return
      } else if (prev) {
        return resolvePage(this.$site.pages, prev, this.$route.path)
      } else {
        return resolvePrev(this.$page, this.sidebarItems)
      }
    },
    next () {
      const next = this.$page.frontmatter.next
      if (next === false) {
        return
      } else if (next) {
        return resolvePage(this.$site.pages, next, this.$route.path)
      } else {
        return resolveNext(this.$page, this.sidebarItems)
      }
    },
    editLink () {
      if (this.$page.frontmatter.editLink === false) {
        return
      }
      const {
        repo,
        editLinks,
        docsDir = '',
        docsBranch = 'master',
        docsRepo = repo
      } = this.$site.themeConfig

      let path = normalize(this.$page.path)
      if (endingSlashRE.test(path)) {
        path += 'README.md'
      } else {
        path += '.md'
      }

      if (docsRepo && editLinks) {
        const base = outboundRE.test(docsRepo)
          ? docsRepo
          : `https://github.com/${docsRepo}`
        return (
          base.replace(endingSlashRE, '') +
          `/edit/${docsBranch}` +
          (docsDir ? '/' + docsDir.replace(endingSlashRE, '') : '') +
          path
        )
      }
    },
    editLinkText () {
      return (
        this.$themeLocaleConfig.editLinkText ||
        this.$site.themeConfig.editLinkText ||
        `Edit this page`
      )
    }
  }
}

function resolvePrev (page, items) {
  return find(page, items, -1)
}

function resolveNext (page, items) {
  return find(page, items, 1)
}

function find (page, items, offset) {
  const res = []
  items.forEach(item => {
    if (item.type === 'group') {
      res.push(...item.children || [])
    } else {
      res.push(item)
    }
  })
  for (let i = 0; i < res.length; i++) {
    const cur = res[i]
    if (cur.type === 'page' && cur.path === page.path) {
      return res[i + offset]
    }
  }
}
</script>

<style lang="stylus">
@import './styles/config.styl'

.page
  padding-bottom 2rem

.edit-link.content
  padding-top 0 !important
  a
    color lighten($textColor, 25%)
    margin-right 0.25rem
  .last-updated
    margin-top: 0 !important
    margin-bottom .5rem
    float right
    font-weight 500
    font-size .9em
    .prefix
      color lighten($textColor, 25%)
    .time
      color #aaa

.page-nav.content
  padding-top 1rem !important
  padding-bottom 0 !important
  .inner
    min-height 2rem
    margin-top 0 !important
    border-top 1px solid $borderColor
    padding-top 1rem
    overflow auto // clear float
  .next
    float right

@media (max-width: $MQMobile)
  .edit-link.content .last-updated
    float none
    text-align left
    margin-top 1rem
    font-size .8em

</style>
