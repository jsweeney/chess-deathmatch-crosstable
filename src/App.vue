<template>
  <div id="app">
    <Match :match="match" />
  </div>
</template>

<script>
import Match from '@/components/Match.vue';

export default {
  name: 'app',
  data() {
    return {
      match: {
        title: 'Jason\'s Happy Hour - playing the field',
        players: ['Jason', 'The field'],
        configuration: '1x10|0, 4x3|0, 10x1|0',
        sections: [{
          timeControl: '10|0',
          games: [
            { color: 'none', result: 'none' },
          ],
        }],
      },
    };
  },
  components: {
    Match,
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
