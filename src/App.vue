<template>
  <Experiment title="UOS - IKW">
    
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
    }
  };
  
</script>

<!-- <style>
.deactivated {
  pointer-events: none;
  background-color: #ffd633;
}

.activated {
    pointer-events: auto;
    background-color: #3333ff;
}

.forced_choice .options .option {
    background-color: #1e1e1e10;
    color: #1e1e1e;
    line-height: 60px;
    height: 60px;
    width: 60px;
    font-size: 32px;
    margin-right: 80px;
    margin-left: 80px;
}

.forced_choice .options .option:hover {
    background-color: #1e1e1e20;
}

.forced_choice .options .correct {
    background-color: #ffd633;
}

.forced_choice .options .wrong {
    background-color: #3333ff;
}
</style> -->