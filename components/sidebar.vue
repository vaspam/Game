<template>
  <section>
    <b-sidebar
      type="is-light"
      :fullheight="fullheight"
      :fullwidth="fullwidth"
      :overlay="overlay"
      :right="right"
      :open.sync="open"
    >
      <a class="delete is-large is-pulled-right is-relative" @click="open = false"></a>

      <div>
        <h3 class="is-size-4">Goal:</h3>
        <p>Score 100 Points</p>
        <br>

        <h1 class="is-size-4">Instructions:</h1>
        <ol>
          <li>Select Card |Sets Card = Choice</li>
          <li>Calculate | Checks Result = Pile</li>
          <li>Add Card | Adds New Card</li>
          <li>Change | Generates New Operator</li>
          <li>Reset | Resets Choices</li>
          <li>Cheat | Randomizes Pile</li>
        </ol>

        <br>

        <h3 class="is-size-4">How:</h3>
        <p>Use +, -, *, /, and Two (2) # Cards to match the # in the Card Pile</p>
        <br>
        <h3 class="is-size-4">Rules:</h3>
        <ul>
          <li>- = 10Points</li>
          <li>+ = 10 Points</li>
          <li>* = 20Points</li>
          <li>/ = 30 points</li>
          <li>Each # card is between 1 and 20</li>
          <li>Successful Matches cause Number Cards to Disappear & 2nd Choice becomes # in Pile</li>
          <li>7 Free actions (except cheat)</li>
          <li>Each action = -1 Point & Draw 1 card</li>
          <li>Cheat = -2 Points</li>
        </ul>

        <br>
        <h3 class="is-size-4">Hint(s):</h3>
        <p>• Valid Pair = Absolute Number Above 0 & Less than 20
          <br>• If Pair Gives a valid total, Hint = Valid Pair
          <br>• If pair is not valid total, Hint = Not playable
          <br>
        </p>
        <br>
      </div>
    </b-sidebar>

    <b-button @click="open = true">Show</b-button>
  </section>
</template>

<script>
export default {
  data() {
    return {
      open: true,
      overlay: true,
      fullheight: true,
      fullwidth: false,
      right: false
    };
  }
};
</script>

<style>
.p-1 {
  padding: 1em;
}
</style>