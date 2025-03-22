<template>
  <div id="app">
    <h1>Birthdate Checker</h1>
    <fieldset style="max-width:300px">
      <legend>Select your gender:</legend>
      <label>
        <input type="radio" value="male" v-model="gender" />
        Male
      </label>
      <label>
        <input type="radio" value="female" v-model="gender" />
        Female
      </label>
    </fieldset>
<br>

    <!-- put gender dependent checkboxes for leagues here -->

    <hr v-if="resultText">
    <div v-if="resultText" class="resultText">
      {{ resultText }}
    </div>
    
  </div>
</template>

<script>
import { ref, computed, watch } from "vue";

export default {
  setup() {
    const gender = ref("");
    const pick = ref("");
    const resultText = ref("");

    watch(pick, () => {
      resultText.value = "";
    });

    watch(gender, () => {
      pick.value = "";
      resultText.value = "";
    })

    const getLeagueDowText = (pick, gender) => {
      if (gender === 'male') {
        switch (pick) {
          case 4: return 'Tee Ball 1';
          case 5: return 'Tee Ball 1';
          case 6: return 'Tee Ball 2';
          case 7: return 'Coach Pitch Baseball (8U)';
          case 8: return 'Coach Pitch Baseball (8U)';
          default: return ''
        }
      } else {
        switch (playingAge) {
          case 4: return 'Tee Ball 1';
          case 5: return 'Tee Ball 1';
          case 6: return 'Tee Ball 2';
          case 7: return 'Coach Pitch Softball (8U)';
          case 8: return 'Coach Pitch Softball (8U)';
          default: return ''
        }
      }

    };

    const game = computed(() => {
      return gender.value === 'male' ? 'Baseball' : 'Softball';
    });

    const getDow = () => {
      if (!gender.value){
        playingAgeResult.value = "Please select a gender."
        return;
      }

      resultText.value = getLeagueDowText(pick.value, gender.value);
    };

    return {
      gender,
      pick,
      resultText
    };
  },
};
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  margin: 20px;
}

#birthdateForm {
  max-width: 700px;
}

.playingAgeResult {
  margin-top: 10px;
  color: #2c3e50;
  font-weight: bold;
}
</style>
