<template>
  <Experiment title="Universität Osnabrück - IKW"
              :image-assets="pictures">

    <!-- PROBABILITY TRIALS -->
    <InstructionScreen>
      <!-- no rule-related language -->
      <div v-if="thisCond() == 1 || thisCond() == 2">
        <p>For this part of the study, you will again choose the category you think each example belongs to. Nun zum letzten Abschnitt der Studie. Wir möchten gerne wissen, wie hoch du die Wahrscheinlichkeit einschätzt, dass das gezeigte Beispiel zu einer der beiden Kategorien gehört:

        <XorProbability
          :response.sync= "$magpie.measurements.prob"
          :initial=50 
          option-left="A"
          option-right="B"/>

        This time you will not receive feedback. </p>
      </div>

      <!-- rule-related language -->
      <div v-else>
          <p>Nun zum letzten Abschnitt der Studie. Wir möchten gerne wissen, wie hoch du die Wahrscheinlichkeit einschätzt, dass das gezeigte Beispiel zu einer der beiden Kategorien gehört, wenn du die zuvor gelernte Regel anwendest:</p>

          <XorProbability
            :response.sync= "$magpie.measurements.prob"
            :initial=50 
            option-left="A"
            option-right="B"/>

          <p>Bla.</p>
      </div>
    </InstructionScreen>

    <!-- PROBABILITY TRIALS -->
    <Screen 
      v-for="(trial, i) in probability"
      :key="i">

        <Record :data="trial" />
        <img :src="trial.image" /> 
        
        <XorProbability
          :response.sync= "$magpie.measurements.prob"
          :initial=50 
          option-left="A"
          option-right="B" />
          
        <div v-if="typeof $magpie.measurements.prob === 'number'">
          <button @click= "$magpie.saveAndNextScreen();">
            Weiter
          </button>
        </div>
    </Screen>

    <!-- Demographics & result submission -->
    <Demographics :education=false :gender=false />

    <SubmitResultsScreen />

  </Experiment>
</template>


<script>
  import _ from 'lodash'
  import XorTraining from './XorTraining'
  import XorGeneralization from './XorGeneralization'
  import XorProbability from './XorProbability'
  import Demographics from './Demographics'
  import raw_training_mixed from '../trials/training-mixed.csv'
  import raw_training_blocked_size from '../trials/training-blocked-size.csv'
  import raw_training_blocked_shape from '../trials/training-blocked-shape.csv'
  import raw_generalization from '../trials/transfer.csv'

  
  // properly shuffled full-random data
  var rnd = []
  for (let i = 0; i < 12; i++) {
    rnd[i] = _.shuffle(raw_training_mixed);
  }
  const training_order_0 = rnd.flat()

  // properly shuffled simple-rule-first data
  var shapesize = _.sample(['shape', 'size']);
  var ord_start = []
  var ord_end = []
  for (let i = 0; i < 4; i++) {
    if (shapesize == 'size') {
      ord_start[i] = _.shuffle(raw_training_blocked_size);
    } else {
      ord_start[i] = _.shuffle(raw_training_blocked_shape);
    }
  }
  for (let i = 0; i < 10; i++) {
    ord_end[i] = _.shuffle(raw_training_mixed);
  }

  const training_order_1 = [ord_start.flat(), ord_end.flat()].flat()
  

  /* debugging & validation area */
  /* for (let i in repeated) {
    console.log("image nr.", i, "name:", repeated[i].image);
  } */

  export default {
    name: 'App',
    components: { XorTraining, XorGeneralization, XorProbability, Demographics },
    data() {
      return {
        pictures: raw_generalization.map(task => task.image),
        train_random: training_order_0,
        train_sorted: training_order_1,
        generalization: _.shuffle(raw_generalization),
        probability: _.shuffle(raw_generalization),
        coin: _.sample(['heads', 'tails'])
      }
    },
    methods: {
      thisChain: function() {
        var chain = this.$magpie.socket.chain
        return chain
      },
      thisCond: function() {
        var condition = this.$magpie.socket.variant
        // console.log("This condition:", condition)
        return condition
      },
      grpIdent: function(coin) {
        const arr = {
          condition: this.$magpie.socket.variant,
          assignment: (coin === 'heads' ? 'correct1' : 'correct2')
        };
        const obj = Object.assign({}, arr);
        return obj
      },
      fsEnter: function() {
        document.documentElement.requestFullscreen();
      }
    }
  };
</script>
<style scoped>
.bouncy {
  width: 20px;
  height: 20px;
  background-color: yellow;
  position: relative;
  animation-name: moove;
  animation-duration: 5s;
  animation-iteration-count: infinite;
  animation-timing-function: ease;
}

@keyframes moove {
  0%    {background-color:#5187ba; left:200px;}
  50%   {background-color:#2696ff; left:700px; transform: rotate(300deg);}
  100%  {background-color:#5187ba; left:200px;}
}
</style>