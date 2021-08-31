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
  computed: {
    gameDescription() {
      return this.game ? this.game.description : "Blank Description"
    },
    gameImage() {
      return this.game ? this.game.image : "https://imageproxy.ifunny.co/crop:x-20,resize:640x,quality:90x75/images/9d2c4c299b4325e2d941d47d0a547e49de03619855033af6f7b279d805e00e46_1.jpg"
    },
    gameTitle() {
      return this.game ? this.game.name : "Blank Title"
    }
  },
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
          content: this.gameTitle
        },
        {
          hid: 'og:description',
          property: 'og:description',
          content: this.gameDescription
        },
        {
          hid: 'og:image',
          property: 'og:image',
          content: this.gameImage
        }
      ]
    }
  },
  async fetch() {
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
