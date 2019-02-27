<template lang="pug">
  .container.sidesblock
    Nav
    h2.jumbotitle {{ getMsg() }}
    router-link(to='/') back to bloggo
</template>

<script>
import Nav from './Nav.vue'
const msgs = [
  `There's nothing here`,
  `How did we get here?`,
  `this is a Four-Oh-Four`,
  `in cyberspace, this is known as a broken link`,
  `this link has moved on. you haven't`,
  `linko no foundo`,
  `a ninja stole this page`,
  `Ahhhhhhhhhhh! This page doesn’t exist`,
  `some horrible error probably occurred`,
  `if you're looking for the internet, sorry bud. nothing here`
]

export default {
  components: { Nav },
  methods: {
    getMsg () {
      return msgs[Math.floor(Math.random() * msgs.length)].toLowerCase()
    }
  }
}
</script>
