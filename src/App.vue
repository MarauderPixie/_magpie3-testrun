<template>
  <Experiment title="Universität Osnabrück - IKW"
              :image-assets="pictures" :wide=true>

    <ConnectInteractiveScreen :title="'Einen Augenblick, bitte.'">
      <p>Es fehlen noch ein paar kleine Schritte zur Vorbereitung des Experiments.</p>
      <br />
      <div class="bouncy"></div>
      <br />
      <p>Danke für die Geduld!</p>
      <br />
      <p>Wenn diese Seite länger als 30 Sekunden angezeigt wird, lade die Seite bitte erneut.</p>
    </ConnectInteractiveScreen> 
    

    <!-- INSTRUCTIONS -->
    <Screen class="welcome">
      <div class="logos"></div>
      <h2>Willkommen</h2>

      <p style="text-align: center;">
        Vielen Dank für die Teilnahme an dieser Studie!
      </p>

      <p>
        Das Ziel dieses Projekts ist, etwas darüber herauszufinden, wie Klassifikationsaufgaben von Menschen erlernt und ausgeführt werden. Dabei handelt es sich um eine Kooperation zwischen der Universität Bremen (Abteilung Allgemeine Psychologie) und der Universität Osnabrück (Institut für Kognitionswissenschaften). Insgesamt wird die Teilnahme etwa 10 bis 15 Minuten in Anspruch nehmen.
      </p>

      <p>
        Die Teilnahme erfolgt vollkommen anonym und freiwillig. Das Experiment kann jederzeit abgebrochen werden. Dazu genügt es, den Browser-Tab bzw. das Fenster zu schließen. In diesem Fall werden keine Daten gespeichert. Datenspeicherung und -übertragung finden erst am Ende des Experiments statt. Studierende der Psychologie und der Cognitive Science der Universitäten Osnabrück und Bremen haben am Ende des Experiments außerdem die Möglichkeit eine halbe Probandenstunde zu erhalten und werden dazu auf eine externe Webseite weitergeleitet.
      </p>

      <button @click="fsEnter()" style="width: 40%;">
        Bitte diesen Button klicken, um in den Vollbild-Modus zu wechseln.
      </button>
      
      <p>
        Mit dem Klick auf den "Weiter"-Button erkläre ich, dass
        <ol>
          <li>Ich einverstanden bin, dass meine Daten anonymisiert auf der Plattform Open Science Framework (osf.io) veröffentlicht werden und im Rahmen von wissenschaftlichen Publikationen (z.B. Vorträge, Veröffentlichung in Fachzeitschriften) verwendet werden. Bitte beachten Sie, dass Sie eine Löschung Ihrer Daten im Nachhinein nicht mehr veranlassen können, da die Daten nur in anonymisierter Form gespeichert werden.</li>
          <li>Ich freiwillig teilnehme und weiß, dass ich die Studie jederzeit ohne Angabe von Gründen abbrechen kann ohne dass mir daraus Nachteile entstehen. In diesem Fall werden keine Daten gespeichert.</li>
          <li>Ich ausreichend über die Studie aufgeklärt wurde.</li>
        </ol>
        Weitere Fragen können per Email an <a href="tanton@uos.de">tanton@uos.de</a> gerichtet werden.
      </p>

      <p style="text-align: center">
        Auf 'weiter' klicken, um mit dem Experiment zu beginnen.
        <button @click= "$magpie.nextScreen();">Weiter</button>
      </p>

      <hr>
      <p class="footer">
        <b>Studienverantwortlicher:</b> Tobias Anton
        <br />
        <b>Addresse:</b> Cognium Gebäude, Raum 0430, Hochschulring 18, 28359 Bremen
        <br />
        <b>Kontakt:</b> <a href="tanton@uos.de">tanton@uos.de</a>
      </p>
    </Screen>

    <InstructionScreen>
      <!-- no rule instructions -->
      <div v-if="thisCond() == 1 || thisCond() == 2">
        <p>Im ersten Abschnitt des Experiments werden dir nacheinander einige Beispiele geometrischer Figuren gezeigt, die sich in ihrer Farbe und Größe unterscheiden. Deine Aufgabe ist es, zu erlernen, ob eine Figur zur Kategorie <b>Nobz</b> oder zur Kategorie <b>Grot</b> gehört:</p>
        <img src="../public/images/e07_1500-900.jpg" 
             style="display:block; margin: -80px auto; position: relative; z-index: -1;">
        <p>Klicke dazu bei jedem Beispiel, das dir gezeigt wird, auf die entsprechende Schaltfläche. Nach jeder Entscheidung bekommst du eine Rückmeldung darüber, ob deine Wahl richtig oder falsch war. Am Anfang musst du raten, letztendlich aber wirst du lernen, wie man die Objekte richtig kategorisiert.</p>
        <p>Gib dein Bestes, um die Kategorien Nobz und Grot zu meistern!</p>
      </div>

      <!-- rule-related language -->
      <div v-else>
        <p>Im ersten Abschnitt des Experiments werden dir nacheinander einige Beispiele geometrischer Figuren gezeigt. Deine Aufgabe ist es, anhand von Farbe und Größe eine Regel zu erlernen, mit der Du entscheiden kannst, ob eine Figur zur Kategorie <b>Nobz</b> oder zur Kategorie <b>Grot</b> gehört:</p>
        <img src="../public/images/e07_1500-900.jpg" 
             style="display:block; margin: -80px auto; position: relative; z-index: -1;">
        <p>Klicke dazu bei jedem Beispiel, das dir gezeigt wird, auf die entsprechende Schaltfläche. Nach jeder Entscheidung bekommst du eine Rückmeldung darüber, ob deine Wahl richtig oder falsch war. Am Anfang musst du raten, letztendlich aber wirst du lernen, wie man die Objekte richtig kategorisiert.</p>
        <p>Gib dein Bestes, um die Regeln für Kategorien Nobz und Grot zu meistern!</p>
      </div>
    </InstructionScreen>


    <!-- TRAINING TRIALS -->
    <Screen 
      v-for="(trial, i) in (thisCond() == 1 || thisCond() == 3 ? train_random : train_sorted)"
      :key="i">

        <Record :data="trial" />  
        <Record :data="grpIdent(coin)" :global=true />
        <img :src="trial.image" /> 
              
        <XorTraining
            :response.sync= "$magpie.measurements.response"
            :options="['Nobz', 'Grot']"
            :correct="(coin === 'heads' ? trial.correct1 : trial.correct2)"
            :feedbackTime=-1 /> 
            
        <p v-if="$magpie.measurements.response">
            <span v-if="$magpie.measurements.response === (coin === 'heads' ? trial.correct1 : trial.correct2)"><b>Korrekt!</b></span> 
            <span v-else><b>Falsch</b></span>
            <button @click= "$magpie.saveAndNextScreen();">Weiter</button>
        </p>
    </Screen>
    

    <InstructionScreen>
      <!-- no rule-related language -->
      <div style="text-align: center;">
      <h3>Gut gemacht!</h3>
        <p>Die Lernphase ist beendet, nun folgt der zweite Teil.</p>
        <p>Die Aufgabe ist die gleiche wie zuvor, nur werden nun alte sowie neue Objekte gezeigt. Dieses mal wirst du jedoch keine Rückmeldung erhalten.</p>
        <p>Wende bitte an, was du im ersten Teil gelernt hast!</p>
      </div>
    </InstructionScreen>


    <!-- TRANSFER TRIALS -->
    <Screen 
      v-for="(trial, i) in generalization"
      :key="i">

        <Record :data="trial" />
        <img :src="trial.image" /> 

        <XorGeneralization
            :response.sync= "$magpie.measurements.response"
            :options="['Nobz', 'Grot']"
            :feedbackTime=-1 /> 
    </Screen>


    <InstructionScreen>
      <!-- no rule-related language -->
      <div>
        <h3>Vielen Dank!</h3>
        <p>Nun zum letzten Abschnitt der Studie. In diesem Durchgang möchten wir gerne wissen, wie hoch du die Wahrscheinlichkeit einschätzt, dass das gezeigte Beispiel zu einer der beiden Kategorien gehört. Verschiebe dazu den Regler in die jeweilige Richtung: </p>

        <XorProbability
          :response.sync="$magpie.measurements.prob"
          :initial=50 
          left="Nobz"
          right="Grot"
          style="width: 70%;"/>
        
        <p>Wenn du denkst, dass beide Kategorien gleich wahrscheinlich sind, belasse den Regler einfach in der Mitte.</p>
      </div>
    </InstructionScreen>

    <!-- PROBABILITY TRIALS -->
    <Screen 
      v-for="(trial, i) in probability"
      :key="i">

        <Record :data="trial" />
        <img :src="trial.image" /> 
        
        <XorProbability
          :response.sync="$magpie.measurements.prob"
          :initial=50 
          left="Nobz"
          right="Grot"/>
        
          <button @click= "$magpie.saveAndNextScreen();">
            Weiter
          </button>
    </Screen>
    

    <!-- Demographics & result submission -->
    <Demographics :education=false :gender=false />

    <SubmitResults />

  </Experiment>
</template>


<script>
  import _ from 'lodash'
  import XorTraining from './XorTraining'
  import XorGeneralization from './XorGeneralization'
  import XorProbability from './XorProbability'
  import Demographics from './Demographics'
  import SubmitResults from './SubmitResults'
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
    components: { XorTraining, XorGeneralization, XorProbability, Demographics, SubmitResults },
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
        var x = document.documentElement;
        /* document.documentElement.requestFullscreen(); */ 
        if (x.requestFullscreen) {
          x.requestFullscreen();
        } else if (x.webkitRequestFullscreen) { /* Safari */
          x.webkitRequestFullscreen();
        } else if (x.msRequestFullscreen) { /* IE11 */
          x.msRequestFullscreen();
        }
      }
    }
  };
</script>
<style>
.logos {
  background-image: url("../public/UOS-Logo_RotFond_sRGB_v01.jpg"), url("../public/logo_ub_2021.png")!important;
  background-repeat: no-repeat !important;
  background-size: contain;
  background-position: left top, right top;
  text-align: left;
  height: 64px;
  width: auto;
  margin-bottom: 20px;
}
.welcome {
  text-align: justify;
  margin: auto;
  width: 70%;
}
.footer {
  text-align: center;
  color: #555E62;
  font-size: 10pt;
}
.vue-slider-process {
  background-color: #ccc;
}
.vue-slider-dot-handle {
  box-shadow: 1px 1px 4px 2px rgba(0, 0, 0, 0.32);
}
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