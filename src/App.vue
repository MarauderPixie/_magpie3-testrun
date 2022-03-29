<template>
  <Experiment title="UOS - IKW">

    <!-- <ConnectInteractiveScreen :title="'sorting into bins...'"></ConnectInteractiveScreen> 

    <Screen>
      <p>Dropping in to see what condition this condition is in:
      <br />
      <b>Variant-Nr.: {{ thisCond() }}</b></p>

      <p>Chain-Maker, Space Lord, Mother, Mother:
      <br />
      <b>Chain-Nr.: {{ thisChain() }}</b></p>
      
      <button @click="$magpie.saveAndNextScreen();">
        Next
      </button>
    </Screen> -->
    

    <!-- INSTRUCTIONS 
    <InstructionScreen :title="'Welcome'">
      Thank you for choosing deutschen Wahn.
    </InstructionScreen> -->

    <!-- <InstructionScreen :title="'Introduction'"> -->
      <!-- no rule-related language -->
      <!-- <div v-if="thisCond() == 1 || thisCond() == 2">
          <p>In this experiment, you will be shown examples of geometric images.
          Your job is to learn to tell whether each example belongs to the <b>Alpha</b> or <b>Beta</b> category.</p>
          
          <p>As you are shown each example, you will be asked to make a category judgment and then you will recieve feedback.
          At first you will have to guess, but you will gain experience as you go along. </p>
          
          <p>After you have completed your training, we will ask you a series of test questions about what you have learned.</p>
      </div> -->

      <!-- rule-related language -->
      <!-- <div v-else>
          <p>In this experiment, you will be shown examples of geometric images.
          Your job is to learn a rule that allows you to tell whether each example belongs to the <b>Alpha</b> or <b>Beta</b> category.</p>

          <p>As you are shown each example, you will be asked to make a category judgment and then you will recieve feedback.
          At first you will have to guess, but you will gain experience as you go along. </p>
          
          <p>Try your best to gain mastery of the Alpha and Beta categories.</p>
      </div>
    </InstructionScreen> -->


    <!-- TRAINING TRIALS -->
    <Screen v-for="(trial, i) in training"
        :key="i">
            
        <img :src="trial.image" /> 
              
        <!-- evtl. muss ich mir "einfach" (haha) eigene buttons bauen -->
        <LockedChoiceInput
            :response.sync= "$magpie.measurements.response"
            :options="['A', 'B']"
            :feedbackTime=-1 /> 
            
        <p v-if="$magpie.measurements.response === trial.correct1">
            <b>Korrekt!</b> 
            <button @click= "$magpie.saveAndNextScreen();">Weiter</button>
        </p>
        <p v-if="$magpie.measurements.response === trial.correct2">
            <b>Falsch!</b> 
            <button @click= "$magpie.saveAndNextScreen();">Weiter</button>
        </p>
    </Screen>
    

    <InstructionScreen :title="'Introduction'">
      <!-- no rule-related language -->
      <div v-if="thisCond() == 1 || thisCond() == 2">
          <p>For this part of the study, you will again choose the category you think each example belongs to. 
          This time you will not receive feedback.</p>
      </div>

      <!-- rule-related language -->
      <div v-else>
          <p>For this part of the study, you will again apply the rule you learned to choose the category you 
          think each example belongs to. This time you will not receive feedback.</p>
      </div>
    </InstructionScreen>


    <!-- GENERALIZATION TRIALS -->
    <Screen v-for="(trial, i) in generalization"
        :key="i">

        <img :src="trial.image" /> 

        <LockedChoiceInput
            :response.sync= "$magpie.measurements.response"
            :options="['A', 'B']"
            :feedbackTime=-1 /> 

        <p v-if="$magpie.measurements.response">
            <button @click= "$magpie.saveAndNextScreen();">Weiter</button>
        </p>
    </Screen>

    <!-- Demographics & result submission -->
    <PostTestScreen />

    <SubmitResultsScreen />

  </Experiment>
</template>


<script>
  import _ from 'lodash'
  import LockedChoiceInput from './LockedChoiceInput'
  import raw_training_full from '../trials/training-full.csv'
  import raw_training_simple from '../trials/training-simple.csv'
  import raw_generalization from '../trials/generalization.csv'

  // prepare simple-rule-first-condition
  const training_order_0 = new Array(12).fill(_.shuffle(raw_training_full)).flat();
  const training_order_1_0 = new Array(4).fill(_.shuffle(raw_training_simple)).flat();
  const training_order_1_1 = new Array(10).fill(_.shuffle(raw_training_full)).flat();
  const training_order_1 = [training_order_1_0, training_order_1_1].flat();

  /* debugging & validation area */
  // console.log("ordered array?", training_order_1)
  
  /* for (let i in repeated) {
    console.log("image nr.", i, "name:", repeated[i].image);
  } */

  export default {
    name: 'App',
    components: { LockedChoiceInput },
    data() {
      return {
        training: training_order_0.slice(0, 8),
        correct:  _.sample([training_order_0.correct1, training_order_0.correct2]),
        generalization: _.shuffle(raw_generalization.slice(1, 3))
      }
    },
    methods: {
      thisChain: function() {
        var chain = this.$magpie.socket.chain
        return chain
      },
      thisCond: function() {
        var condition = this.$magpie.socket.variant
        // console.log("This condition has rll:", condition == 3 || condition == 4)
        return condition
      }
    }
  };
  
</script>
