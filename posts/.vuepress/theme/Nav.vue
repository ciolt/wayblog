<template lang="pug">
  nav
    router-link.logoblock(to='/', :class="$route.path === '/' ? 'selected' : ''") &nbsp;bloggo&nbsp;
    ul.navver
      li(v-for='link in navblocks', :key='link.id')
        span &nbsp;
        router-link(:to='link.href', :class="$route.path === link.href ? 'selected' : ''") &nbsp;{{ link.title }}&nbsp;
</template>

<script>
export default {
  components: { },
  data () {
    return {
      navblocks: [
        { href: '/posts.html', title: 'posts' },
        { href: '/about.html', title: 'about' }
      ]
    }
  }
}
</script>
