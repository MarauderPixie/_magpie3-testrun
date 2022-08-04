<template>
  <Screen v-if="!$magpie.debug" title="Fertig.">
    <Slide>
      <p>Einen Augenblick, Daten werden übermittelt.</p>
      <Wait :time="0" @done="submit(() => $magpie.nextSlide())" />
    </Slide>
    <Slide>
      <div v-if="!error">
        <p>
          Vielen lieben Dank für die Teilnahme!
          <Wait :time="3000" @done="redirectToCompletionUrl" />
        </p>
        <p>
          Cognitive Science Studierende der Uni Osnabrück und Psychologie Studierende der Uni Bremen können <a href="https://marauderpixie.github.io/ma-thesis-vp-hours/" target="_blank">diesem Link folgen</a>, um eine halbe Probandenstunde zu erhalten.
        </p>
        <p>Dieses Fenster kann nun geschlossen werden.</p>
      </div>
      <div v-else>
        <p>Oh nein! Es gab ein Problem bei der Datenübermittlung.</p>
        <p>
          Bitte kontaktiere
          <a :href="'mailto:' + $magpie.contactEmail"
            >den Autor des Experiments.</a
          >.
        </p>
        <p v-text="error" />
      </div>
    </Slide>
  </Screen>
  <DebugResultsScreen v-else />
</template>

<script>
import Screen from '../node_modules/magpie-base/src/components/Screen';
import Wait from '../node_modules/magpie-base/src/components/helpers/Wait';
import DebugResultsScreen from '../node_modules/magpie-base/src/components/screens/DebugResultsScreen';
import Slide from '../node_modules/magpie-base/src/components/Slide';
export default {
  name: 'SubmitResults',
  components: { Slide, DebugResultsScreen, Wait, Screen },
  props: {},
  data() {
    return {
      error: null
    };
  },
  methods: {
    async submit(cb) {
      try {
        await this.$magpie.submit();
        cb();
      } catch (err) {
        this.error = err.message;
        cb();
      }
    },
    redirectToCompletionUrl() {
      if (this.$magpie.completionUrl && this.$magpie.mode === 'prolific') {
        window.location = this.$magpie.completionUrl;
      }
    }
  }
};
</script>
