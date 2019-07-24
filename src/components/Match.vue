<template>
  <div class="">
    <nav class="nav-extended red darken-4">
      <div class="marquee">The Chess DeathMatch Crosstable 2000 Pro</div>
    </nav>
    <div class="container">
      <div class="match">
        <div class="title">{{match.title}}</div>
        <div class="crosstable">
          <div class="players crosstable-element">
            <div class="players-label">&nbsp;</div>
            <div
              class="player"
              v-for="player in match.players"
              :key="player"
            >
              <span class="player">{{player}}</span>
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
                      v-bind:class="{ white: game.color === 'white',
                                      black: game.color === 'black' }"
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
                      v-bind:class="{ black: game.color === 'white',
                                      white: game.color === 'black' }"
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
      <div class="configuration card red darken-2">
        <div class="card-content white-text">
          <span class="card-title">Configuration</span>
          <p>Configure your death match in this panel.
            Simply type in the information in the various fields.</p>
        </div>
        <div class="form-wrapper">
          <div class="form-content card white">
            <div class="row">
              <form class="col s12">
                <div class="row">
                  <div class="input-field col s12">
                    <input
                      id="title"
                      type="text"
                      v-model="match.title"
                    >
                    <label
                      class="active"
                      for="title"
                    >Title</label>
                  </div>
                </div>
                <div class="row">
                  <div class="input-field col s6">
                    <input
                      id="firstName"
                      type="text"
                      v-model="match.players[0]"
                    >
                    <label
                      class="active"
                      for="first_name"
                    >First Name</label>
                  </div>
                  <div class="input-field col s6">
                    <input
                      id="lastName"
                      type="text"
                      v-model="match.players[1]"
                    >
                    <label
                      class="active"
                      for="last_name"
                    >Last Name</label>
                  </div>
                </div>
                <div class="row">
                  <div class="col s12">
                    Time control and number of games in each section:
                    <div class="input-field inline">
                      <input
                        id="configuration"
                        type="text"
                        v-model="match.configuration"
                        @keyup="configureMatch"
                        style="width: 250px"
                      >
                      <span class="helper-text">e.g.: 3 x 5|5, 10 x 2|1</span>
                    </div>
                  </div>
                </div>
              </form>
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
    configureMatch() {
      this.match.sections = [];
      const sections = [];
      const sectionConfigs = this.match.configuration.split(',').map(config => config.trim());
      sectionConfigs.forEach((sectionConfig) => {
        if (sectionConfig.includes('x')) {
          const gameCount = sectionConfig.split('x')[0].trim();
          const timeControl = sectionConfig.split('x')[1].trim();
          const section = { timeControl, games: [] };
          for (let gameIndex = 0; gameIndex < gameCount; gameIndex += 1) {
            section.games.push({ color: 'none', result: 'none' });
          }
          sections.push(section);
        }
      });
      this.match.sections = sections;
    },
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
.marquee {
  font-size: 2em;
  color: white;
  text-align: center;
}
.title {
  padding: 20px 0;
  font-size: 3em;
  color: black;
  text-align: center;
}
.match {
  padding: 50px 0;
}
.crosstable {
  padding: 20px 0;
  text-align: center;
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
  padding: 0;
  margin: 0 5px;
}
.time-control {
  padding-bottom: 2px;
  text-align: center;
}
.game {
  display: inline-block;
  width: 30px;
  text-align: center;
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
.form-wrapper {
  padding: 0 10px 1px;
}
.form-content {
  padding: 15px;
}
.row {
  margin-bottom: 0px;
}
</style>
