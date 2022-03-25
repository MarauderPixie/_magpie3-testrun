<template>
  <Experiment title="UOS - IKW">

    <!-- <ConnectInteractiveScreen :title="'sorting into bins...'"></ConnectInteractiveScreen> 

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
  import raw_training from '../trials/training.csv'
  import raw_generalization from '../trials/generalization.csv'
  

  export default {
    name: 'App',
    components: { LockedChoiceInput },
    data() {
      return {
        training: _.shuffle(raw_training.slice(1, 7)),
        generalization: _.shuffle(raw_generalization.slice(1, 3))
      };
    },
    methods: {
      thisGen: function() {
        var generation = this.$magpie.socket.generation
        return generation
      },
      thisCond: function() {
        var condition = this.$magpie.socket.variant
        // console.log("This condition has rll:", condition == 3 || condition == 4)
        return condition
      }
    }
  };
  
</script>
