<template>
  <Screen v-bind="$attrs" title="Weitere Angaben">
    <Slide>
      <p>
        Die folgenden Fragen zu beantworten ist völlig optional, wird uns jedoch bei der Analyse der Ergebnisse weiterhelfen.
      </p>
      <div style="text-align: center; width: auto; margin: 0 auto">
        <p v-if="age">
          <label>
            Dein Alter (in Jahren): 
            <input
              v-model="$magpie.measurements.age"
              type="number"
              max="110"
              min="18" />
          </label>
        </p>
        <p v-if="gender">
          <label>
            Gender: <DropdownInput
                      :options="['', 'male', 'female', 'other']"
                      :response.sync="$magpie.measurements.gender" />
          </label>
        </p>
        <p v-if="education">
          <label
            >Dein höchster Bildungsabschluss:
            <DropdownInput
              orientation="horizontal"
              :options="[
                '(keine Angabe)',
                'kein Abschluss',
                'Haupt- oder Volksschule',
                'Realschule / mittlere Reife',
                '(Fach-)Gymnasium / Abitur',
                'Berufsausbildung',
                'Hochschule (Bachelor, Vordiplom)',
                'Hochschule (Master, Diplom)',
                'Promotion'
              ]"
              :response.sync="$magpie.measurements.education"
            />
          </label>
        </p>
        <p v-if="languages">
          <label>
            Muttersprache(n):
            <input
              v-model="$magpie.measurements.languages"
              type="text"
              placeholder="deutsch, englisch, latein..."
            />
          </label>
        </p>
        <hr>
        <!-- @slot You can add additional questions here, storing data in measurements
           @binding {object} measurements a temporary object to store your responses before adding them to the results
           -->
        <slot :measurements="$magpie.measurements" />
        <p>
          <b>Beschreibe bitte, wie du bei der Kategorisierung vorgegangen bist:</b>
          <TextareaInput
            :response.sync="$magpie.measurements.strategy"
          />
        </p>
        <hr>
        <p>
          <label>
            <p><b>Gibt es einen Grund aus dem wir deine Daten nicht auswerten sollten?</b> <br /> (z.B. längere Unterbrechung, Unkonzentriertheit, laute Umgebung, etc...) </p>
            <MultipleChoiceInput
              :options="['Ja (Daten werden entfernt)', 'Nein (Daten werden behalten)']"
              orientation="horizontal"
              :response.sync="$magpie.measurements.usage" />
          </label>
          <p>Dir entsteht kein Nachteil, diese Frage zu ehrlich beantworten! <br />Die Zuverlässigkeit unserer Daten ist uns jedoch sehr wichtig.</p>
        </p>
        <hr>
        <p>
          Hast du noch weitere Anmerkungen? Wir freuen uns über jegliches Feedback!
          <TextareaInput
            :response.sync="$magpie.measurements.comments"
          />
        </p>
      </div>

      <div>
        <hr>
        <p>
          Durch klicken auf 'Beenden' wird der Vollbildmodus verlassen und deine Daten werden übermittelt.
          <button
            @click="
              $magpie.addExpData($magpie.measurements);
              $magpie.nextScreen();
              fsExit();
            "
          >
            Beenden
          </button>
        </p>
      </div>
    </Slide>
  </Screen>
</template>

<script>
export default {
  name: 'Demographics',
  props: {
    /**
     * Whether to ask for participant age
     */
    age: {
      type: Boolean,
      default: true
    },
    /**
     * Whether to ask for participant gender
     */
    gender: {
      type: Boolean,
      default: true
    },
    /**
     * Whether to ask for participant education
     */
    education: {
      type: Boolean,
      default: true
    },
    /**
     * Whether to ask for participant mother tongues
     */
    languages: {
      type: Boolean,
      default: true
    },
    /**
     * Whether to ask for comments
     */
    comments: {
      type: Boolean,
      default: true
    }
  },
  methods: {
    fsExit: function() {
      document.exitFullscreen();
    }
  }
};
</script>
<style>
  textarea {
    width: 400px;
    height: 72px;
    border-radius: 2px !important;
    padding: 7px 10px !important;
  }
</style>
