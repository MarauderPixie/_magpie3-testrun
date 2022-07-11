<template>
  <Screen v-bind="$attrs" title="Weitere Angaben">
    <Slide>
      <p>
        Die folgenden Fragen zu beantworten ist völlig optional, wird uns jedoch bei der Analyse unserer Ergebnisse weiterhelfen.
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
            >Level of Education
            <DropdownInput
              :options="[
                '',
                'Graduated High-school',
                'Graduated College',
                'Higher degree'
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
              placeholder="Java, C++, ..."
            />
          </label>
        </p>
        <hr>
        <!-- @slot You can add additional questions here, storing data in measurements
           @binding {object} measurements a temporary object to store your responses before adding them to the results
           -->
        <slot :measurements="$magpie.measurements" />
        <p>
          Beschreibe bitte die Regel oder Strategie, nach der du zur Kategorisierung vorgegangen bist:
          <TextareaInput
            :response.sync="$magpie.measurements.strategy"
          />
        </p>
        <hr>
        <p>
          <label>
            Gibt es einen Grund, weswegen wir deine Daten nicht auswerten sollten? (z.B. Ablenkungen, Unterbrechungen, Ernsthaftigkeit...) 
            <MultipleChoiceInput
              :options="['yes', 'no']"
              orientation="horizontal"
              :response.sync="$magpie.measurements.usage" />
          </label>
          Bitte sei ehrlich! 'Ja' anzukreuzen hat keinerlei negative Auswirkungen für dich (du erhältst zB trotzdem Probandenstunden), aber hilft uns dafür, zuverlässige Daten zu sammeln!
        </p>
        <hr>
        <p>
          Hast du noch weitere Anmerkungen? Wir freuen uns über jedes Feedback!
          <TextareaInput
            :response.sync="$magpie.measurements.comments"
          />
        </p>
      </div>

      <div>
        <hr>
        <p>
          Wenn du auf den Knopf klickst, beendest du das Experiment - der Vollbildmodus wird verlassen und deine Daten übermittelt.
          <button
            @click="
              $magpie.addExpData($magpie.measurements);
              $magpie.nextScreen();
              fsExit();
            "
          >
            Beenden & Absenden
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
