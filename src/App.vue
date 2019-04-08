<template>

  <div id="app" class="container">

    <h1 class="mb-4 btn btn-primary">A quiz ?</h1>

    <b-alert v-if="end" show class="col-4 offset-4">Your Score is : {{ score }}/{{ questions.length }}</b-alert>

    <div class="row">

      <div class="col-md-6 offset-3">

        <b-card :header="questions[index].question" header-tag="header">

          <b-list-group>

            <b-list-group-item button v-for="(quiz , index) in questions[index].answers" :key="quiz.id" @click="action(index)" :variant="variants[index]">
              {{ quiz }}
            </b-list-group-item>

          </b-list-group>

          <b-button v-if="end" @click="start_over" class="mt-4">Start Over !</b-button>

          <b-button v-if="trueAnswer && !end" @click="lets_go_on" class="mt-4">Continue</b-button>

        </b-card>

      </div>

    </div>

  </div>

</template>

<script>

export default {
  name: 'app',
    data () {
        return {
            index: 0,
            score: 0,
            end: false,
            variants: [...Array(4)],
            trueAnswer: false
        }
    },

    methods: {
        action (index) {
            // test the correct answer.
            if (index === this.questions[this.index].ok) {
                this.score++;
            } else {
                this.variants[index] = 'danger';
            }

            this.trueAnswer = true;
            this.variants[this.questions[this.index].ok] = 'success';
            // Quiz end of test.
            if (this.index === this.questions.length - 1) {
                this.end = true;
            }
        },

        start_over () {
            this.trueAnswer = this.end = this.index = this.score = 0;
            this.variants = [...Array(4)];
        },

        lets_go_on () {
            this.trueAnswer = false;
            this.variants = [...Array(4)];
            this.index++;
        }
    },

    computed: {
      questions () {
          return this.$store.state.questions;
      }
    },

    created () {
      return this.$store.dispatch('getData');
    }


}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  background-image: url("./assets/body.jpg");
  background-repeat: no-repeat;
  background-size: cover;
}
</style>
