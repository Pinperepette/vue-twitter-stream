<script>
  export default {
    name: 'Controls',
    data: () => ({
      chips: [],
      chipsPlaceholder: 'Enter tags to track',
      chipsShortPlaceholder: '+Tag',
      value: ''
    }),
    methods: {
      addChip() {
        this.chips.push(this.value.replace(/^(#)/, ''))
        this.value = ''
      },
      removeChip(index) {
        this.chips.splice(index, 1)
      },
      removePrevChip() {
        if (!this.value.length) {
          this.chips.splice(-1, 1)
        }
      },
      focus() {
        this.$refs.tagsInput.focus()
      }
    },
    computed: {
      limitReached() {
        return this.chips.length >= 5
      },
      placeholderText() {
        if (this.limitReached) {
          return ''
        }

        if (this.chips.length) {
          return this.chipsShortPlaceholder
        }

        return this.chipsPlaceholder
      }
    }
  }
</script>

<template>
  <div>
    <div class="row">
      <div class="col s12">
        <div class="card white">
          <div class="card-content center-align black-text">
            <span class="card-title cyan-text">Choose Twitter Tags</span>

            <blockquote>
              Enter up to five tags below and we'll stream live data from the
              <a class="btn-flat" href="https://dev.twitter.com/streaming/overview" target="_blank">Twitter API</a>
              to visualize it with the power of <a class="btn-flat" href="https://vuejs.org" target="_blank">Vue</a> and <a class="btn-flat" href="http://www.chartjs.org" target="_blank">Chart.js</a> for you. Enjoy <span class="red-text text-lighten-2">&hearts;</span>
            </blockquote>

            <div class="row answers left-align">
              <div class="col s12 m2 hide-on-small-only"></div>
              <div class="col s12 m8" @click="focus">
                <div class="chips chips-placeholder">
                  <div class="chip" v-for="(chip, index) in chips">
                    #{{ chip }}<i class="material-icons close" @click="removeChip(index)">close</i>
                  </div>
                  <input
                    class="input"
                    ref="tagsInput"
                    :readonly="limitReached"
                    :placeholder="placeholderText"
                    @keydown.enter="addChip"
                    @keydown.delete="removePrevChip"
                    v-model="value"
                  >
                </div>
              </div>
              <div class="col s12 m2 hide-on-small-only"></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .chips {
    margin-top: 1rem;
  }

  .chips>input {
    width: 125px !important;
  }

  .card blockquote {
    max-width: 36rem;
    margin: 15px auto 25px auto;
    line-height: 1.7;
  }

  .card blockquote a.btn-flat {
    margin: 0;
    height: 20px;
    padding: 0 5px;
    line-height: 20px;
    margin-top: -2px;
  }
</style>