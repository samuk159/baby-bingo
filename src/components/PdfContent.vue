<template>
  <section class="pdf-content">
    <div v-for="(cartela, indexCartela) in cartelas" :key="'cartela_' + indexCartela" class="borda-cartela">
      <div class="cartela-container">
        
        <div class="cartela bg-white mx-2 p-1">
          <div v-for="(linha, indexLinha) in cartela" :key="'linha_' + indexLinha" class="row py-1 mx-n1">
            <div v-for="(coluna, indexColuna) in linha" :key="'coluna_' + indexColuna" class="col text-center px-1">

              <div class="cell-container text-center align-middle" :class="{ 'letra-azul': azulIndexes[indexLinha].includes(indexColuna), 'letra-rosa': rosaIndexes[indexLinha].includes(indexColuna) }">
                {{coluna}}
              </div>

            </div>
          </div>
        </div>

      </div>
    </div>
  </section>
</template>

<script>
//import { palavras } from '../palavras.js'
import {} from '../palavras.js'

export default {
  data() {
    return {
      cartelas: [],
      palavras: [],
      numeroDeCartelas: 4,
      palavrasPorLinha: 4,
      azulIndexes: [
        [0, 2],
        [1, 3],
        [0, 2],
        [1, 3]
      ],
      rosaIndexes: [
        [1, 3],
        [0, 2],
        [1, 3],
        [0, 2]
      ]
    };
  },

  mounted() {
    this.palavras = window.palavras;
    console.log(this.palavras.length);
    this.generateCartelas();

    this.$nextTick(() => {
      setTimeout(() => {
        this.$emit("domRendered");
      }, 1000);
    });
  },

  methods: {
    generateCartelas() {
      for (let i = 0; i < this.numeroDeCartelas; i++) {
        let palavrasPorCartela = Math.pow(this.palavrasPorLinha, 2);
        let currentWords = [];

        while (currentWords.length < palavrasPorCartela) {
          let word = this.getRandomWord();

          if (!currentWords.includes(word)) {
            currentWords.push(word);
          }
        }

        let cartela = [];
        let currentLinhaIndex = 0;
        for (let j = 0; j < currentWords.length; j++) {
          let word = currentWords[j];

          if (!cartela[currentLinhaIndex]) {
            cartela[currentLinhaIndex] = [];
          }

          cartela[currentLinhaIndex].push(word);

          if (cartela[currentLinhaIndex].length >= this.palavrasPorLinha) {
            currentLinhaIndex++;
          }
        }

        this.cartelas.push(cartela);
      }

      console.log(this.cartelas);
    },

    getRandomWord() {
      return this.palavras[this.getRandomInt(0, this.palavras.length - 1)];
    },

    getRandomInt(min, max) {
      min = Math.ceil(min);
      max = Math.floor(max);
      return Math.floor(Math.random() * (max - min + 1)) + min;
    }
  }
};
</script>

<style lang="scss" scoped>
.borda-cartela {
  padding-top: 5px !important;
  padding-bottom: 5px !important;
  padding-left: 5px !important;
  padding-right: 10px !important;
}

.cartela-container {
  background-image: url("../assets/background.png");
  background-size: 785px;
  height: 364px;
  padding-top: 75px;
}

.pdf-content {
  width: 100%;
  background: #fff;
}

.cell-container {
  border: 3px dashed #ccc;
  height: 60px;
  line-height: 55px;
  font-family: BabyPumpkin;
  font-size: 30px;
  overflow: hidden;
}

.letra-rosa {
  color: #ed45d9;
}

.letra-azul {
  color: #45a4ed;
}
</style>