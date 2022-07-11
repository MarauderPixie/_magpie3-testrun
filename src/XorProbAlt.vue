<template>
  <!-- pass down props -->
  <LifecycleScreen v-bind="$props">
    <!-- pass down slots -->
    <template slot="fixation">
      <slot name="fixation"></slot>
    </template>
    <template slot="stimulus">
      <slot name="stimulus"></slot>
    </template>

    <template #task>
      <Record
        :data="{
          optionLeft,
          optionRight
        }"
      />
      <!-- <p v-if="question" v-text="question"></p> -->
      <SliderInput
        :left="optionLeft"
        :right="optionRight"
        :min="min"
        :max="max"
        :interval="interval"
        :initial="initial"
        :disabled="disabled"
        :response.sync="$magpie.measurements.prob"
        :id="slidar"
      />
      <p v-if="!isNaN($magpie.measurements.prob)" style="width: 50%; float: left; margin: 0;">{{ 100 - $magpie.measurements.prob }}%</p>
      <p v-if="!isNaN($magpie.measurements.prob)" style="width: 50%; float: right; margin: 0;">{{ 100 - (100 - $magpie.measurements.prob) }}% </p>
      <button
        v-if="
          $magpie.measurements.prob &&
          !$magpie.validateMeasurements.$invalid
        "
        @click="nextAfterResponse"
      >
        Next
      </button>
    </template>
    <template #feedback>
      <slot name="feedback"></slot>
    </template>
  </LifecycleScreen>
</template>

<script>
import Record from '../node_modules/magpie-base/src/components/helpers/Record';
import SliderInput from '../node_modules/magpie-base/src/components/inputs/SliderInput';
import LifecycleScreen from '../node_modules/magpie-base/src/components/screens/LifecycleScreen';

/**
 * Inherits from LifecycleScreen
 */
export default {
  name: 'XorProbability',
  components: {
    LifecycleScreen,
    SliderInput,
    Record
  },
  extends: LifecycleScreen,
  props: {
    /* Left label for rating */
    optionLeft: {
      type: String,
      default: ''
    },
    /* Right label for rating */
    optionRight: {
      type: String,
      default: ''
    },
    /* Initial slider position */
    initial: {
      type: Number,
      optional: true,
      default: 0
    },
    /* Minimal slider value */
    min: {
      type: Number,
      optional: true,
      default: 0
    },
    /* Maximum slider value */
    max: {
      type: Number,
      optional: true,
      default: 100
    },
    /* Interval between slider values */
    interval: {
      type: Number,
      optional: true,
      default: 1
    },
    disabled: {
      type: Boolean,
      optional: true,
      default: false
    }
  },
  data() {
      return {
        pointerEvents: 'auto'
      };
    },
  computed: {
    compPointer: function() {
      return this.pointerEvents;
    }
  },
  methods: {
    onOptionClick(bla) {
      /**
       * Change event with the chosen option. Useful in combination with `response.sync`
       */
      this.$emit('update:response', prob);
      this.pointerEvents = 'none';
      // this.backgroundColor = 'blue';
    }
  }
};
</script>
<style scoped>
.slider {
  width: 50%;
  display: block;
  margin: auto;
}
.slider .slider-element {
  margin: 20px 0 auto;
}
</style>
