<template>
  <div>
    <Header />
    <main>
      <nuxt />
    </main>
    <Footer />
  </div>
</template>

<script>
const header = () => import('@/components/header')
const footer = () => import('@/components/footer')

export default {
  components: {
    Header: header,
    Footer: footer
  }
}
</script>

<style lang="scss" scoped>
main {
  min-height: calc(100vh - 80px);
}
</style>
