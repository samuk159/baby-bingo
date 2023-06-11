<template>
  <section class="pdf-content">
    <div v-for="(cartela, indexCartela) in cartelas" :key="'cartela_' + indexCartela" style="padding: 20px !important;">
      <div class="cartela-container">
        
        <div class="cartela bg-white mx-3 p-1">
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
export default {
  data() {
    return {
      cartelas: [],
      palavras: ['Babador', 'Chupeta', 'Banheira', 'Manta', 'Berço', 'Fralda', 'Papinha', 'Carrinho', 'Macacão', 'Sapatinho', 'Chocalho', 'Mamadeira', 'Naninha', 'Pelúcia', 'Enxoval', 'Leite', 'Brinquedo', 'Algodão', 'Sabonete', 'Lencinho', 'Mamãe', 'Papai', 'Titio', 'Titia', 'Vovô', 'Vovó', 'Móbile', 'Moisés', 'Colinho', 'Body', 'Mordedor'],
      numeroDeCartelas: 3,
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
.cartela-container {
  background-image: url("../assets/background.png");
  background-size: 760px;
  height: 520px;
  padding-top: 75px;
}

.pdf-content {
  width: 100%;
  background: #fff;
}

.cell-container {
  border: 3px dashed #ccc;
  height: 98px;
  line-height: 98px;
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