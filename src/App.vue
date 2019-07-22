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
        title: 'Bertrout v. JPS - The Canuck revenge',
        players: [
          { twitch: 'bertrout', chesscom: 'bertrout' },
          { twitch: 'jasonpierresweeney', chesscom: 'jasonpsweeney' },
        ],
        sections: [{
          timeControl: '10|0',
          games: [
            { color: 'none', result: 'none' },
            { color: 'none', result: 'none' },
          ],
        }, {
          timeControl: '3|0',
          games: [
            { color: 'none', result: 'none' },
            { color: 'none', result: 'none' },
            { color: 'none', result: 'none' },
            { color: 'none', result: 'none' },
          ],
        }, {
          timeControl: '1|0',
          games: [
            { color: 'none', result: 'none' },
            { color: 'none', result: 'none' },
            { color: 'none', result: 'none' },
            { color: 'none', result: 'none' },
            { color: 'none', result: 'none' },
            { color: 'none', result: 'none' },
            { color: 'none', result: 'none' },
            { color: 'none', result: 'none' },
            { color: 'none', result: 'none' },
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
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
