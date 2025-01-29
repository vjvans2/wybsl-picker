<template>
  <div id="app">
    <h1>Birthdate Checker</h1>
    <!-- Gender Radio Buttons -->
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
    <form id="birthdateForm" @submit.prevent="checkBirthdate">
      <label for="birthdate">Enter the players birthdate: </label><br>
      <input type="date" v-model="birthdate" required :max="maxDate" :min="minDate"/>&nbsp;&nbsp;
      <button type="submit">Submit</button>
    </form>
    <hr v-if="playingAgeResult">
    <div v-if="playingAgeResult" class="playingAgeResult">
      {{ playingAgeResult }}
    </div>
    <div v-if="playingAgePrimary" class="playingAgeResult">
      <span>Primary Recommended {{ game }} League -- {{ playingAgePrimary }}</span>
    </div>
    <div v-if="playingAgeSecondary" class="playingAgeResult">
      <span>Secondary Recommended {{ game }} League -- {{ playingAgeSecondary }}</span>
    </div>
  </div>
</template>

<script>
import { ref, computed, watch } from "vue";

export default {
  setup() {
    const birthdate = ref("");
    const gender = ref("");
    const playingAgeResult = ref("");
    const playingAgePrimary = ref("");
    const playingAgeSecondary = ref("");

    watch(gender, () => {
      playingAgeResult.value = "";
      playingAgePrimary.value = "";
      playingAgeSecondary.value = "";
    });

    const calculatePlayingAge = (birthday) => {
      const today = new Date();
      let age = today.getFullYear() - birthday.getFullYear();

      // Adjust playing age if birthday is after April (month index starts at 0)
      if (birthday.getMonth() + 1 > 4) {
        age -= 1;
      }

      return age;
    };

    const getPrimaryLeague = (playingAge, gender) => {
      // Add logic to determine the primary league based on playing age
      if (gender === 'male') {
        switch (playingAge) {
          case 4: return 'Tee Ball 1';
          case 5: return 'Tee Ball 1';
          case 6: return 'Tee Ball 2';
          case 7: return 'Coach Pitch Baseball (8U)';
          case 8: return 'Coach Pitch Baseball (8U)';
          case 9: return 'Green Hat Kid Pitch Baseball (10U) ';
          case 10: return 'Green Hat Kid Pitch Baseball (10U) ';
          case 11: return 'Red Hat Kid Pitch Baseball (12U) ';
          case 12: return 'Red Hat Kid Pitch Baseball (12U) ';
          case 13: return 'Blue Hat Kid Pitch Baseball (14U) ';
          case 14: return 'Blue Hat Kid Pitch Baseball (14U) ';
          case 15: return 'Black Hat Kid Pitch Baseball (18U) ';
          case 16: return 'Black Hat Kid Pitch Baseball (18U) ';
          case 17: return 'Black Hat Kid Pitch Baseball (18U) ';
          default: return ''
        }
      } else {
        switch (playingAge) {
          case 4: return 'Tee Ball 1';
          case 5: return 'Tee Ball 1';
          case 6: return 'Tee Ball 2';
          case 7: return 'Coach Pitch Softball (8U)';
          case 8: return 'Coach Pitch Softball (8U)';
          case 9: return 'Green Hat Kid Pitch Softball (10U) ';
          case 10: return 'Green Hat Kid Pitch Softball (10U) ';
          case 11: return 'Red Hat Kid Pitch Softball (12U) ';
          case 12: return 'Red Hat Kid Pitch Softball (12U) ';
          case 13: return 'Blue Hat Kid Pitch Softball (17U) ';
          case 14: return 'Blue Hat Kid Pitch Softball (17U) ';
          case 15: return 'Blue Hat Kid Pitch Softball (17U) ';
          case 16: return 'Blue Hat Kid Pitch Softball (17U) ';
          case 17: return 'Blue Hat Kid Pitch Softball (17U) ';
          default: return ''
        }
      }

    };

    const getSecondaryLeague = (playingAge, gender) => {
      if (gender === 'male') {
        switch (playingAge) {
          case 5: return 'Tee Ball 2 (if played Tee Ball 1 prior)';
          case 6: return 'Coach Pitch Baseball (8U) (with successful tryout)';
          case 7: return 'Green Hat Kid Pitch Baseball (10U) (with successful tryout)';
          case 8: return 'Green Hat Kid Pitch Baseball (10U) (with successful tryout)';
          case 9: return 'Red Hat Kid Pitch Baseball (12U) (with successful tryout)';
          case 10: return 'Red Hat Kid Pitch Baseball (12U) (with successful tryout)';
          case 11: return 'Blue Hat Kid Pitch Baseball (14U) (with successful tryout)';
          case 12: return 'Blue Hat Kid Pitch Baseball (14U) (with successful tryout)';
          case 13: return 'Black Hat Kid Pitch Baseball (18U) (with successful tryout)';
          case 14: return 'Black Hat Kid Pitch Baseball (18U) (with successful tryout)';
          default: return ''
        }
      } else {
        switch (playingAge) {
          case 5: return 'Tee Ball 2 (if played Tee Ball 1 prior)';
          case 6: return 'Coach Pitch Softball (8U) (with successful tryout)';
          case 7: return 'Green Hat Kid Pitch Softball (10U) (with successful tryout)';
          case 8: return 'Green Hat Kid Pitch Softball (10U) (with successful tryout)';
          case 9: return 'Red Hat Kid Pitch Softball (12U) (with successful tryout)';
          case 10: return 'Red Hat Kid Pitch Softball (12U) (with successful tryout)';
          case 11: return 'Blue Hat Kid Pitch Softball (17U) (with successful tryout)';
          case 12: return 'Blue Hat Kid Pitch Softball (17U) (with successful tryout)';
          default: return ''
        }
      }
    };

    const game = computed(() => {
      return gender.value === 'male' ? 'Baseball' : 'Softball';
    });

    const maxDate = computed(() => {
      const today = new Date();
      return `${today.getFullYear() - 3}-12-31`;
    });

    const minDate = computed(() => {
      return "2000-01-01";
    });

    const checkBirthdate = () => {
      if (!birthdate.value) {
        playingAgeResult.value = "Please enter a valid date.";
        return;
      }

      if (!gender.value){
        playingAgeResult.value = "Please select a gender."
        return;
      }

      const birthday = new Date(birthdate.value);
      if (isNaN(birthday)) {
        playingAgeResult.value = "Invalid date format.";
        return;
      }

      const playingAge = calculatePlayingAge(birthday);
      playingAgeResult.value = `Your playing age is ${playingAge}`;
      playingAgePrimary.value = getPrimaryLeague(playingAge, gender.value);
      playingAgeSecondary.value = getSecondaryLeague(playingAge, gender.value);
    };

    return {
      birthdate, gender,
      playingAgeResult,
      playingAgePrimary,
      playingAgeSecondary,
      checkBirthdate, game, maxDate, minDate
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
