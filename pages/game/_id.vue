<template>
  <div>
    <div v-if="!game">Loading Game...</div>
    <div v-else>
      <h1>{{game.name}}</h1>
      <p>{{game.description}}</p>
      <h1>Can Play Solo? {{game.canPlaySolo ? "✅" : "❌"}}</h1>
      <img :src="game.image" alt="" />
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    game: null
  }),
  head() {
    return {
      title: "Title?",
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Home page description'
        },
        {
          hid: 'og:title',
          property: 'og:title',
          content: this.game?.title
        },
        {
          hid: 'og:description',
          property: 'og:description',
          content: this.game?.description
        },
        {
          hid: 'og:image',
          property: 'og:image',
          content: this.game?.image
        }
      ]
    }
  },
  async mounted() {
    console.log(`id: ${this.$route.params.id}`)
    let docSnap = await this.$fire.firestore.collection("games").doc(this.$route.params.id).get()
    if (docSnap.exists) {
      console.log("Got the game!")
      this.game = docSnap.data();
    } else {
      console.error('game with that id does not exist')
    }
  }
}
</script>

<style></style>
