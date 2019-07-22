<template>
  <div class="match">
    <div class="marquee">
      <h1>{{match.title}}</h1>
    </div>
    <div class="crosstable">
      <div class="players crosstable-element">
        <div class="players-label">&nbsp;</div>
        <div
          class="player"
          v-for="player in match.players"
          :key="player.twitch"
        >
          <span class="player">{{player.twitch}}</span>
        </div>
      </div>
      <div class="sections crosstable-element">
        <div
          class="section"
          v-for="(section, sectionIndex) in match.sections"
          :key="sectionIndex"
        >
          <div
            class="time-control"
            @click.exact="colorSection(sectionIndex)"
            @click.shift="clearSection(sectionIndex)"
          >{{section.timeControl}}</div>
          <div class="games">
            <div
              class="game"
              v-for="(game, gameIndex) in section.games"
              :key="gameIndex"
              @click="scoreGame(sectionIndex, gameIndex)"
            >
              <div class="top">
                <div
                  class="board"
                  v-bind:class="{ white: game.color === 'white', black: game.color === 'black' }"
                >
                  <span v-if="game.result === 'win'">1</span>
                  <span v-else-if="game.result === 'loss'">0</span>
                  <span v-else-if="game.result === 'draw'">&half;</span>
                  <span v-else>-</span>
                </div>
              </div>
              <div class="bottom">
                <div
                  class="board"
                  v-bind:class="{ black: game.color === 'white', white: game.color === 'black' }"
                >
                  <span v-if="game.result === 'win'">0</span>
                  <span v-else-if="game.result === 'loss'">1</span>
                  <span v-else-if="game.result === 'draw'">&half;</span>
                  <span v-else>-</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'Match',
  props: {
    match: Object,
  },
  methods: {
    colorSection(sectionIndex) {
      const section = this.match.sections[sectionIndex];
      let color;
      switch (section.games[0].color) {
        case 'none': {
          color = 'white';
          break;
        }
        case 'white': {
          color = 'black';
          break;
        }
        default:
          color = 'none';
      }
      const gamesLength = this.match.sections[sectionIndex].games.length;
      for (let gameIndex = 0; gameIndex < gamesLength; gameIndex += 1) {
        this.match.sections[sectionIndex].games[gameIndex].color = color;
        switch (color) {
          case 'white': {
            color = 'black';
            break;
          }
          case 'black': {
            color = 'white';
            break;
          }
          default:
            color = 'none';
        }
      }
    },
    clearSection(sectionIndex) {
      const gamesLength = this.match.sections[sectionIndex].games.length;
      for (let gameIndex = 0; gameIndex < gamesLength; gameIndex += 1) {
        this.match.sections[sectionIndex].games[gameIndex].color = 'none';
        this.match.sections[sectionIndex].games[gameIndex].result = 'none';
      }
    },
    scoreGame(sectionIndex, gameIndex) {
      const game = this.match.sections[sectionIndex].games[gameIndex];
      switch (game.result) {
        case 'none': {
          game.result = 'win';
          break;
        }
        case 'win': {
          game.result = 'loss';
          break;
        }
        case 'loss': {
          game.result = 'draw';
          break;
        }
        default:
          game.result = 'none';
      }
      this.match.sections[sectionIndex].games[gameIndex] = game;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
.crosstable-element {
  display: inline-block;
}
.players {
  padding: 0 5px;
  text-align: right;
}
.sections {
  cursor: pointer;
  user-select: none;
}
.section {
  display: inline-block;
  margin: 0 5px;
}
.time-control {
  padding-bottom: 2px;
}
.game {
  display: inline-block;
  width: 30px;
}
.top .board {
  background: linear-gradient(to right bottom, #666, #fff);
  border: 1px solid #666;
}
.bottom .board {
  background: linear-gradient(to right bottom, #666, #fff);
  border: 1px solid #666;
}
.board.white {
  background: white;
  color: #333;
  border: 1px solid #333;
}
.board.black {
  background: #333;
  color: white;
  border: 1px solid #333;
}
</style>
