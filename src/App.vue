<template>
  <Experiment title="UOS - IKW">

    <ConnectInteractiveScreen :title="'sorting into bins...'"></ConnectInteractiveScreen>

    <Screen>
      <p>Method acting:</p>
      <br />
      <b>{{ thisGen() }}</b>
      
      <p>Dropping in to see what condition this condition is in:</p>
      <br />
      <b>{{ thisCond() }}</b>

      <button @click="$magpie.saveAndNextScreen();">
        Next
      </button>
    </Screen>
    
    <!-- <InstructionScreen :title="'Welcome'">
      Thank you for choosing deutschen Wahn.
    </InstructionScreen>

    <InstructionScreen :title="'Introduction'">
      <p>In this experiment, you will be shown examples of geometric images.
      Your job is to learn to tell whether each example belongs to the <b>Alpha</b> or <b>Beta</b> category.</p>
      
      <p>As you are shown each example, you will be asked to make a category judgment and then you will recieve feedback.
      At first you will have to guess, but you will gain experience as you go along. </p>
      
      <p>After you have completed your training, we will ask you a series of test questions about what you have learned.</p>
    </InstructionScreen> -->


    <!-- TRAINING TRIALS -->
        <Screen v-for="(trial, i) in training"
        :key="i">
            
        <img :src="trial.image" /> 
              
        <!-- evtl. muss ich mir "einfach" (haha) eigene buttons bauen -->
        <LockedChoiceInput
            :response.sync= "$magpie.measurements.category"
            :options="['A', 'B']"
            :feedbackTime=-1 /> 
            
        <p v-if="$magpie.measurements.category === trial.correct1">
            <b>Korrekt!</b> 
            <br />
            <button @click= "$magpie.saveAndNextScreen();">Weiter</button>
        </p>
        <p v-if="$magpie.measurements.category === trial.correct2">
            <b>Falsch!</b> 
            <br />
            <button @click= "$magpie.saveAndNextScreen();">Weiter</button>
        </p>
    </Screen>


    <!-- GENERALIZATION TRIALS -->
    <Screen v-for="(trial, i) in generalization"
        :key="i">

        <img :src="trial.image" /> 

        <LockedChoiceInput
            :response.sync= "$magpie.measurements.category"
            :options="['A', 'B']"
            :feedbackTime=-1 /> 

        <p v-if="$magpie.measurements.category">
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
  import training from '../trials/training.csv'
  import generalization from '../trials/generalization.csv'
  

  export default {
    name: 'App',
    components: { LockedChoiceInput },
    data() {
      return {
        training: _.shuffle(training),
        generalization: _.shuffle(generalization)
      };
    },
    methods: {
      thisGen: function() {
        var generation = this.$magpie.socket.generation

        if (generation == 1) {
          console.log("1st Gen!")
        } else {
          console.log("Next Gen!")
        }

        return generation
      },
      thisCond: function() {
        var condition = this.$magpie.socket.variant
        return condition
      }
    }
  };
  
</script>
