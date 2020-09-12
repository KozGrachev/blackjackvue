<!--          KNOWN-ISSUES

    1.  When the player gets a soft hand and stands, the higher
        number should be compared with the dealer's score

    2.  Sometimes when DEAL is pressed, only one card is visible in the player's hand.

-->



<!------------------------------------------------------------------------------------------------------------>
<!--


                                           T E M P L A T E


                                                                                                   -->
<!------------------------------------------------------------------------------------------------------------>

<template>
  <div id="visible-area">
    <div id="window">
      <section id="dealer-stats-container">
        <p class="score-indicator" id="dealer-score">{{dealerScore()}}</p>
      </section>

      <section id="dealer-container" class="card-area">
        <div class="hand-container" id="dealer-hand">
          <div v-for="(data, card) in dealerHand" :key="card">
            <div class="card-div">
              <img class="card-img" :src="getImgUrl(data[0])" :alt="data[0]" />
            </div>
          </div>
        </div>
      </section>

      <!-- TODO: implement betting. Player should choose from a range of chips which, when
                 clicked, should go to the bet-container section (preferably with animation
      -->
      <section id="bet-container"></section>

      <section id="player-stats-container">
        <p class="score-indicator" id="player-score">{{playerScore()}}</p>
      </section>

      <section id="player-container" class="card-area">
        <div class="hand-container" id="player-hand">
          <div v-for="(data, card) in playerHand" :key="card">
            <div class="card-div">
              <img class="card-img" :src="getImgUrl(data[0])" :alt="data[0]" />
            </div>
          </div>

          <!-- TODO: finish implementing the hint card. It should poke out from underneath
                     the players cards and when clicked, tell the player whether to hit or
                     to stand, refering to the strategy chart.
          -->
          <!-- <div v-if="cardsDealt" class="card-img" id="hint-card">
            <h1 class="menu-text" id="gameComment"></h1>
          </div>-->
        </div>
      </section>

      <div class="message" id="end-round" v-if="endOfRound">
        <h1>{{gameComment}}</h1>
      </div>

      <section id="controls-container">
        <div id="controls">
          <button id="stand-button" @click="stand" v-bind:disabled="!cardsDealt">STAND</button>
          <button id="deal-button" @click="deal" v-bind:disabled="cardsDealt==true">DEAL</button>
          <button id="hit-button" @click="hit" v-bind:disabled="!cardsDealt">HIT</button>
        </div>
      </section>

      <div id="bank-container">
        <p id="bank-display">Bank: {{bank}}</p>
      </div>
    </div>
  </div>
</template>






<!------------------------------------------------------------------------------------------------------------>
<!--


                                           S T Y L E


                                                                                                   -->
<!------------------------------------------------------------------------------------------------------------>



<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@500&display=swap");

#visible-area {
  background: #264653;
  width: 1fr;
  height: 1fr;
}

#window {
  font-family: "Montserrat", sans-serif;
  color: #264653;
  position: absolute;
  width: auto;
  height: 98%;
  display: grid;
  grid-template-columns: 150px minmax(400px, 1000px) 150px;
  grid-template-rows: 1.5em 30% auto 30% auto auto;
  grid-template-areas:
    ". . ."
    ". dealer dealer"
    "bet . ."
    "score player ."
    "controls controls controls"
    "bank bank bank";
  /* background-image: linear-gradient(-30deg, #E9C46A, #F4A261); */
  /* border: 3px solid grey; */
  background: red/* #70e1f5; /* fallback for old browsers */
  /* background: linear-gradient(
    135deg,
    #DBD5AD,
    #9EDCD5
  ) */
  ;

  border-radius: 3em;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  box-shadow: -0.5em 0.5em 2em -1em;
}

.message {
  grid-area: player;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 105%;
  height: 120%;
  background: #264653dd;
  display: flex;
  justify-content: center;
  align-items: center;
  align-content: center;
}

#dealer-container {
  grid-area: dealer;
}
#bet-container {
  grid-area: bet;
}

#player-stats-container {
  display: flex;
  grid-area: score;
  width: 100%;
  height: 100%;
  align-content: center;
  /* border: 3px solid grey; */
}

#player-container {
  grid-area: player;
  display: flex;
  /* justify-content: center; */
}
#controls-container {
  width: inherit;
  grid-area: controls;
  display: flex;
  justify-content: center;
  align-items: flex-end;
  /* border: 3px solid red; */
  /* background: #2a9d8f; */
  /* background-clip:padding-box; */
}

.score-indicator {
  display: flex;
  position: relative;
  width: 50px;
  height: 50px;
  top: -10%;
  left: 50%;
  transform: translate(-50%);
  border-radius: 50%;
  background: #e76f51;
  background: #2a9d8f;
  color: white;
  text-align-last: center;
  justify-content: center;
  font-size: 200%;
  /* padding: 10%; */
  line-height: 1.6;
}

#dealer-score {
  background: #d16a66;
}

.card-area {
  display: flex;
  align-content: space-around;
  /* pos */
  margin: 0px 28%;
  transform: translateX(-50%);
}
.hand-container {
  position: relative;

  height: 1fr;
  display: flex;
  justify-content: center;

  /* border: 3px solid blue; */
}

#bank-container {
  grid-area: bank;
  display: flex;
  justify-content: baseline;
  align-content: center;
  /* border: 3px solid blue; */
  background: #264653;
  border-radius: 3em 0em 3em 3em;
  /* box-shadow: 10px 10px 10px 5px pink inset */
}
#bank-display {
  position: absolute;
  /* bottom: -1%; */
  left: 50%;
  transform: translateX(-50%);
  line-height: 0px;
  font-size: 300%;
  color: white;
}
.card-img {
  position: relative;
  height: 100%;
  filter: drop-shadow(-3px 1px 5px #7f916b77);
}
.card-div {
  /* margin:0% 100%; */
  height: 100%;
  width: 3em;
  z-index: 10;
}

/* TO BE IMPLEMENTED LATER */
/* #hint-card{
    position: absolute;
    height: 99%;
    width: 160px;
    left: 20em;
    z-index: 1;
    background: #FBEFD0;
    border: 0.01em solid black;
    border-radius: 1em;
  } */

h1 {
  /* padding: 1em; */
  /* position: relative; */
  /* margin: 50%; */
  padding: 10%;
  position: absolute;
  font-size: 350%;
  /* background: linear-gradient(60deg, #2a9d8f, #cca647); */
  /* background-clip: text; */
  /* color: transparent; */
  color: #a1d9e7;
}

button {
  background-color: white;
  border: 5px solid transparent;
  font-family: "Montserrat", sans-serif;
  padding: 10px;
  width: 100px;
  position: relative;
  height: 50px;
  transition: background-color 500ms, color 100ms;
}

button:disabled:hover {
  background-color: white;
}
button:focus {
  outline: none;
}
button:active {
  /* outline: 10px solid greenyellow; */
  border: 1px solid grey;
  /* border-width: 10px; */
  transition: border 100ms ease-out;
}

button:hover {
  background-color: rgb(241, 241, 241);
}

#stand-button {
  border-radius: 5em 0 0 0em;
}

#hit-button {
  border-radius: 0em 5em 0 0;
}

#controls {
  /* padding: 25%; */
  display: flex;
  /* border: 5px solid blue; */
  justify-content: space-around;
}
</style>



<!------------------------------------------------------------------------------------------------------------>
<!--


                                           S C R I P T


                                                                                                   -->
<!------------------------------------------------------------------------------------------------------------>

<script>
export default {
  name: "Game",
  data() {
    class Deck {
      constructor() {
        this._allCards = {
          // Card names code:
          // aceH = ace of hearts, fiveC = 5 of clubs, queenS = queen of spades, 10D = ten of diamonds

          // HEARTS
          "2H": 2,
          "3H": 3,
          "4H": 4,
          "5H": 5,
          "6H": 6,
          "7H": 7,
          "8H": 8,
          "9H": 9,
          TH: 10,
          JH: 10,
          QH: 10,
          KH: 10,
          AH: 1,

          // DIAMONDS
          "2D": 2,
          "3D": 3,
          "4D": 4,
          "5D": 5,
          "6D": 6,
          "7D": 7,
          "8D": 8,
          "9D": 9,
          TD: 10,
          JD: 10,
          QD: 10,
          KD: 10,
          AD: 1,

          // SPADES
          "2S": 2,
          "3S": 3,
          "4S": 4,
          "5S": 5,
          "6S": 6,
          "7S": 7,
          "8S": 8,
          "9S": 9,
          TS: 10,
          JS: 10,
          QS: 10,
          KS: 10,
          AS: 1,

          // CLUBS
          "2C": 2,
          "3C": 3,
          "4C": 4,
          "5C": 5,
          "6C": 6,
          "7C": 7,
          "8C": 8,
          "9C": 9,
          TC: 10,
          JC: 10,
          QC: 10,
          KC: 10,
          AC: 1,
        };
        this._talon = []; // where cards are dealt from
        this._usedCards = []; // used cards pile
        this.resetDeck(); // creates a full talon (52 cards) and an empty usedCards pile
        this._playerHand = []; //
        this._dealerHand = [];
      }

      get playerHand() {
        return this._playerHand;
      }

      get dealerHand() {
        return this._dealerHand;
      }

      get talon() {
        return this._talon;
      }

      resetDeck() {
        const arr = [];
        for (const [key, value] of Object.entries(this._allCards)) {
          arr.push([key, value]);
        }
        this._talon = arr;
        this._usedCards = [];
      }

      checkTalon() {
        // if no cards left in talon, move all the used cards to the talon
        // console.log(`talon length: ${this.talon.length}`);
        if (this._talon.length < 1) {
          console.log(
            "\n-----------------------------------------------------------------"
          );
          console.log("SHUFFLING CARDS");
          console.log(
            "-----------------------------------------------------------------\n"
          );
          this.moveCards(this._usedCards, this._talon);
        }
      }

      moveCards(fromArr, toArr) {
        while (fromArr.length > 0) {
          // console.log(this._playerHand)
          toArr.push(fromArr.pop());
        }
      }

      deal(arr, numCards) {
        // this.newRound();

        // deal a set number of randomly picked cards to the player or the dealer (does not work on other arrays)
        if (arr == this._playerHand || arr == this._dealerHand) {
          for (let i = 0; i < numCards; i++) {
            this.checkTalon();
            const cardIndex = Math.floor(Math.random() * this.talon.length);
            const card = this.talon[cardIndex];
            arr.push(card);
            this._talon.splice(cardIndex, 1);
          }
        } else
          console.log(
            "ERROR: invalid card recipient. Can only deal to player or dealer."
          );
      }

      dealPlayer(numCards) {
        // give n random cards to player
        // console.log(`DEALING ${numCards} CARDS TO PLAYER`);
        this.deal(this._playerHand, numCards);
        // console.log(`player's cards:`);
        // console.log(this._playerHand);
      }

      dealDealer(numCards) {
        // give n random cards to dealer
        // console.log(`DEALING ${numCards} CARDS TO DEALER`);
        this.deal(this._dealerHand, numCards);
        // console.log(`dealer's cards:`);
        // console.log(this._dealerHand);
      }

      newRound() {
        // at the end of a round, take the cards from the player and the dealer and put them with the usedCards cards
        console.log("STARTING NEW ROUND");
        this.moveCards(this._playerHand, this._usedCards);
        this.moveCards(this._dealerHand, this._usedCards);

        //FIXME: why aren't there any used cards after this method??
        console.log(`Used cards:`);
        console.log(this._usedCards);
      }

      handCount(hand) {
        let count = [0, 0];
        // let handHasAce = false;

        for (const card of hand) {
          const value = card[1];

          if (value == 1) {
            count[0] += 1;
            count[1] += 10;
          } else {
            count[0] += value;
          }

          if (count[0] + count[1] > 21) {
            count[1] = 0;
          }
        }

        return count;
      }
    }

    this.deck = new Deck();

    // Vue variables
    // this.cardsDealt = false;

    return {
      cardPath: "./cards_svg/aceH.svg",
      cardsDealt: false,
      endOfRound: false,
      playerHand: this.deck.playerHand,
      dealerHand: this.deck.dealerHand,
      // gameActive: this.gameActive,
      gameComment: "Let's play!",
      bank: 2000,
      buttonsDisabled: false,
      // return count[1] > 0 ? count[0] + "/" + count[0] + count[1] : count[0];
    };
  },

  methods: {
    getImgUrl(card) {
      // require.context ensures that the cards_svg folder is loaded at compile time
      let images = require.context("./cards_svg/", false, /\.svg$/);
      return images("./" + card + ".svg");
    },

    deal() {
      this.deck.newRound();
      this.deck.dealDealer(1);
      this.deck.dealPlayer(2);
      this.cardsDealt = true;
      this.endOfRound = false;
      this.gameComment = "Hit or Stand?";
      this.evaluate("deal");
    },

    evaluate(action) {
      //FIXME: When split score, and player stands, make the displayed score the maximum

      const totalD = this.dealerTotal();
      const totalP = this.playerTotal();
      console.log("Dealer total: " + totalD);

      switch (action) {
        case "stand":
          if (this.playerTotal[1] > 0) {
            totalP[0] = totalP[0] + totalP[1];
          }

          if (totalD[0] > 21 || totalD[0] < totalP[0]) {
            this.endRound("win");
          } else if (totalD[0] > totalP[0]) {
            this.endRound("lose");
          } else if (totalD[0] == totalP[0]) {
            this.endRound("draw");
          }
          break;

        case "hit":
          if (totalP[0] > 21) {
            this.endRound("bust");
          } else if (totalP[0] == 21) {
            this.endRound("blackjackP");
          }
          break;

        case "deal":
          // if player has blackjack, call blackjack!
          if (totalP[1] == 10 && totalP[0] == 11) this.endRound("blackjackP");
          break;

        default:
          console.log(
            "ERROR: Invalid Evaluation. Cannot evaluate action: " + action
          );
          break;
      }
    },

    hit() {
      console.log("HIT");
      this.deck.dealPlayer(1);
      console.log(this.playerTotal());

      this.evaluate("hit");
    },

    stand() {
      console.log("STAND");
      while (this.dealerTotal()[0] < 17) {
        this.deck.dealDealer(1);
        console.log(this.dealerTotal());
      }

      this.evaluate("stand");
      // TODO: Put endRound outcomes to the commentary
    },

    playerTotal() {
      // display single digit or "5/15", depending on whether there is an ace
      const total = this.deck.handCount(this.deck.playerHand);
      return total;
    },

    dealerTotal() {
      // total is a number unless there is an ace in the hand, when it becomes an array
      const total = this.deck.handCount(this.deck.dealerHand);
      return total;
    },

    dealerScore() {
      return this.score(this.dealerTotal());
    },

    playerScore() {
      return this.score(this.playerTotal());
    },

    score(total) {
      const sum = total[0] + total[1];
      return total[1] > 0 ? total[0] + "/" + sum : total[0];
    },

    endRound(outcome) {
      this.endOfRound = true;
      switch (outcome) {
        case "win":
          console.log("***YOU WIN***");
          this.gameComment = "YOU WIN\n🏆";
          this.bank += 100;
          break;

        case "lose":
          console.log("***YOU LOSE***");
          this.gameComment = "YOU LOSE\n💩";
          this.bank -= 100;
          break;

        case "bust":
          console.log("***BUST***");
          this.gameComment = "BUST!\n💀";
          this.bank -= 100;
          break;

        case "draw":
          console.log("***PUSHED***");
          this.gameComment = "PUSHED\n👉";
          break;

        case "blackjackP":
          console.log("***BLACKJACK***");
          console.log("***YOU WIN***");
          this.gameComment = "♠BLACKJACK♠\nYOU WIN\n🎉";
          this.bank += 100;
          break;

        case "blackjackD":
          console.log("***BLACKJACK***");
          console.log("YOU LOSE");
          this.gameComment = "♠BLACKJACK♠\nYOU LOSE\n😖";
          this.bank -= 100;
          break;

        default:
          console.log("ERROR: Unknown Round Outcome");
          break;
      }
      this.cardsDealt = false;
    },
  },
};
</script>