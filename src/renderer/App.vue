<template>
<div id="calculator" class="calculator">

  <header class="header">
    <input v-model="output" class="output" type="text" readonly>
    <p class="output-text">{{ outputText }}</p>
  </header>

  <main class="main">

    <section class="numbers">

      <button
          v-for="(number, index) in numbers"
          :key="`number-${ index }`"
          :class="{
            'numbers__button': Number.isInteger(number),
            'numbers__action': !Number.isInteger(number)
          }"
          type="button"
          @click="handleNumberClick(number)">
        {{ number }}
      </button>

    </section>

    <aside class="actions">

      <button
          v-for="(action, index) in actions"
          :key="`action-${ index }`"
          class="actions__button"
          type="button"
          @click="handleActionClick(action)">
        {{ action }}
      </button>

    </aside>

  </main>

  <footer class="footer">
    <button type="button" @click="reset">C</button>
    <button :disabled="!operation" type="button" @click="submit">=</button>
  </footer>

</div>
</template>

<script>
export default {
  name: 'App',

  filters: {
    checkOutput: value => value || 0
  },

  data () {
    return {
      numbers: [7, 8, 9, 4, 5, 6, 1, 2, 3, '.', 0, '+/-'],
      actions: ['+', '-', '*', '/'],
      clearOutput: false,
      clearOutputText: false,
      outputText: '',
      operation: null,
      input: 0,
      output: 0
    }
  },

  methods: {
    handleNumberClick (value) {
      if (this.clearOutput) {
        this.output = 0
        this.clearOutput = false
      }
      if (Number.isInteger(value)) {
        this.output = (this.output == 0)
          ? Number(value.toString())
          : Number(this.output + value.toString())
      }
      else switch (value) {
        case '.':
          break
        case '+/-':
          this.output *= (-1)
          break
        default: break
      }
    },

    handleActionClick (value) {
      this.operation = value
      this.input = this.output
      this.clearOutput = true

      if (this.clearOutputText) {
        this.outputText = ` ${ this.output } ${ value }`
        this.clearOutputText = false
      }
      else {
        this.outputText += ` ${ this.output } ${ value }`
      }
    },

    submit () {
      this.outputText += ` ${ this.output }`

      switch (this.operation) {
        case '+':
          this.output = this.input + this.output
          break
        case '-':
          this.output = this.input - this.output
          break
        case '*':
          this.output = this.input * this.output
          break
        case '/':
          this.output = this.input / this.output
          break
        default: break
      }

      this.clearOutputText = true
      this.reset(false)
    },

    reset (full = true) {
      this.clearOutput = true
      this.operation = null
      this.input = 0
      if (full) {
        this.output = 0
        this.outputText = ''
      }
    }
  }
}
</script>

<style src="./assets/reboot.css"></style>

<style lang="sass">
\:root
  --main-container-width: 22.5rem
  --main-button-width: 5rem
  --main-element-height: 4rem
  --main-margin: .5rem
  --main-padding: .5rem

html
  font-size: 10px

input,
button
  height: var(--main-element-height)
  font-size: 2em
</style>


<style lang="sass" scoped>
@mixin main_padding_without ($position)
  padding: var(--main-padding)
  padding-#{$position}: 0

.main,
.header,
.footer
  width: var(--main-container-width)

.calculator
  display: grid

  max-width: 20rem

  font-family: menu

.main
  display: grid

  grid-template-columns: calc(var(--main-button-width) * 3 + var(--main-padding) * 3) calc(var(--main-button-width) + var(--main-padding))
  grid-gap: var(--main-padding)

.header
  display: grid

  @include main_padding_without(bottom)

.output
  width: calc(var(--main-container-width) - var(--main-padding) * 2)
  padding: var(--main-padding)
  text-align: right

.output-text
  margin-bottom: 0
  font-size: 1.6rem
  &:empty
    display: none

.numbers
  display: grid

  grid-template-columns: repeat(3, var(--main-button-width))
  grid-gap: var(--main-padding)
  grid-column: 1 / 2
  grid-row: 1

  @include main_padding_without(right)

.actions
  display: grid

  grid-template-columns: 1
  grid-gap: var(--main-padding)
  grid-column: 2 / 2
  grid-row: 1

  @include main_padding_without(left)

.footer
  display: grid
  width: max-content

  grid-template-columns: 2

  @include main_padding_without(top)

  > button
    width: calc(var(--main-button-width) * 2 + var(--main-padding))
    grid-row: 1

    &:first-child
      margin-right: var(--main-margin)
</style>

