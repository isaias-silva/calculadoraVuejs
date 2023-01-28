<template>
  <div class="calculator">
    <input type="text" name="formule" class='formulainput' v-model="formula">
    <div class="block symbols">
      <Button v-on:click="concatFormule(objButton.symbol)" v-for="(objButton, key) in buttonsInfo" :key="key" :symbol="objButton.symbol" :title="objButton.title" />
    </div>
    <div class="block numbers" >
      <Button v-on:click="concatFormule(objButton.toString())" v-for="(objButton, key) in numbers" :key="key" :symbol="objButton.toString()" :title="objButton.toString()" />
      <Button symbol="calcular" v-on:click="calculate()" />
      <Button symbol="limpar" v-on:click="clear()" />
    </div>
    <div class="block aux">
      <Button v-on:click="concatFormule(objButton.symbol)" v-for="(objButton, key) in buttonsAux" :key="key" :symbol="objButton.symbol" :title="objButton.title" />
    </div>
  </div>
</template>

<script lang="ts">
import Button from './components/Button.vue'
import { defineComponent } from 'vue'

export default defineComponent({
  components: {
    Button
  },
  data (): { buttonsInfo: { title: string, symbol: string }[], buttonsAux: { title: string, symbol: string }[], numbers: number[], formula:string|null|undefined } {
    return {
      buttonsInfo: [
        {
          title: 'soma',
          symbol: '+'
        },
        {
          title: 'subtração',
          symbol: '-'
        },
        {
          title: 'divisão',
          symbol: '/'
        },
        {
          title: 'multiplicação',
          symbol: '*'
        }

      ],
      buttonsAux: [
        {
          title: 'potencia',
          symbol: '^'
        },
        {
          title: 'parentese',
          symbol: '('
        },
        {
          title: 'parentese',
          symbol: ')'
        },
        {
          title: 'raiz',
          symbol: '√'
        }

      ],
      numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 0],
      formula: null
    }
  },
  methods: {
    concatFormule (value:string) {
      this.formula = this.formula ? this.formula + value : value
    },
    calculate () {
      try {
        let stringEval = this.formula ? this.formula : ''
        const sqrtPos = stringEval.search(/√/g)
        if (sqrtPos !== -1) {
          const quadrado = stringEval.substring(sqrtPos)
          const newQuadrado = quadrado.replace(/√/g, 'Math.sqrt(') + ')'
          stringEval = stringEval.replace(quadrado, newQuadrado)
        }
        stringEval = stringEval.includes('^') ? stringEval.replace(/\^/, '**') : stringEval
        // eslint-disable-next-line no-eval
        this.formula = eval(stringEval)
      } catch (err: unknown) {
        console.log(err)
        this.clear()
      }
    },
    clear () {
      this.formula = null
    }
  }
})
</script>
<style>
*{
  margin:0;
  padding: 0;
}
.calculator{
  width: 40%;
  border:1px solid black;
  margin: auto;
  position: relative;
  box-shadow: 10px 10px 10px black;

}
.calculator .block.symbols{
  display: flex;
  width: 100%;
}
.calculator .block.numbers{
  display: grid;
  width: 100%;
  grid-template-columns: 25% 25% 25% ;
  grid-template-rows: 25% 25% 25% ;
}

.block button{
  width: 100%;
  height: 60px;
}

.formulainput{
  width: 99%;
  height: 40px;
  font-size: 24px;
}
.aux{
  display: flex;
  width: 25%;
  position: absolute;
  top:30%;
  right: 0;
  flex-direction: column;
  height: 100%;
}
</style>
