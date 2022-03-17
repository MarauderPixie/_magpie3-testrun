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
    </InstructionScreen>


    let's try experimental trials -->
    <Screen v-for="(trial, i) in trialData"
      :key="i">
        
        <img :src="trial.images" /> 
          
        <!-- evtl. muss ich mir "einfach" (haha) eigene buttons bauen
        <ForcedChoiceInput
          :response.sync= "$magpie.measurements.category"
          :options="['A', 'B']"
          :feedbackTime=-1
          @click="isActive = !isActive" 
          :class="{correct:isActive}" /> -->
        <button 
        class="option" 
        :class="{correct:isActive}"
        @click="isActive = !isActive"> A </button>

        <button 
        class="option" 
        :class="{wrong:isActive}"
        @click="isActive = !isActive"> B </button>
        
        <p v-show="$magpie.measurements.category === 'A'">
            Korrekt! <br >
            <button @click= "$magpie.saveAndNextScreen();">Weiter</button>
        </p>
        <p v-show="$magpie.measurements.category === 'B'">
            Falsch! <br >
            <button @click= "$magpie.saveAndNextScreen();">Weiter</button>
        </p>
        <!--  :class="{'correct': $magpie.measurements.category === 'A'}" -->
          
    </Screen>
    

    <!-- Demographics & result submission -->
    <PostTestScreen />

    <SubmitResultsScreen />

  </Experiment>
</template>


<script>
  import trialData from '../trials/stimulist.csv';
  import _ from 'lodash';

  export default {
    name: 'App',
    data() {
      return {
        trialData,
        isActive: false
      };
    }
  };
  
</script>

<style>
.option {
    background-color: #1e1e1e10;
    color: #1e1e1e;
    line-height: 60px;
    height: 60px;
    width: 60px;
    font-size: 32px;
    margin-right: 80px;
    margin-left: 80px;
}

.option:hover {
    background-color: #1e1e1e20;
}

.correct {
    background-color: #ffd633;
}

.wrong {
    background-color: #3333ff;
}
</style>