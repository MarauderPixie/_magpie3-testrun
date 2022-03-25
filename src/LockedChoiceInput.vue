<template>
  <div class="forced_choice">
    <div class="options">
      <div
        class="option"
        v-for="option in options"
        :key="option"
        v-bind:style="{ pointerEvents: compPointer }" 
        @click="onOptionClick(option)"
      >
        <div
          @click="bgClick()" v-bind:style="{ backgroundColor: compBackground }" 
        > 
          {{ option }} 
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/**
 * Have the participant choose between multiple options.
 */
export default {
  name: 'LockedChoiceInput',
  props: { 
    /**
     * The possible options to choose from
     */
    options: {
      type: Array,
      required: true
    }
  },
  data() {
      return {
        backgroundColor: '',
        pointerEvents: 'auto'
      };
    },
  computed: {
    compBackground: function() {
      return this.backgroundColor;
    },
    compPointer: function() {
      return this.pointerEvents;
    }
  },
  methods: {
    onOptionClick(option) {
      /**
       * Change event with the chosen option. Useful in combination with `response.sync`
       */
      this.$emit('update:response', option);
      this.pointerEvents = 'none';
    },
    bgClick() {
      this.backgroundColor = 'blue';    
    }
  }
};
</script>

<style scoped>
.deactivated {
    /* background-color: #ffd633 !important; */
    pointer-events: none;
}

.activated {
    /* background-color: #3333ff !important; */
    pointer-events: auto;
}

.option {
  background-color: #1e1e1e10;
  border: none;
  border-radius: 2px;
  color: #1e1e1e;
  cursor: pointer;
  display: inline-block;
  font-family: 'Lato', 'Noto Sans', sans-serif;
  font-size: 40px;
  line-height: 80px;
  height: 80px;
  width: 80px;
  font-weight: 700;
  letter-spacing: 0.9px;
  margin: 0 100px;
  outline: none;
  padding: 5px 10px;
  text-transform: uppercase;
}

.option:hover {
  background-color: #1e1e1e20;
}
</style>
