<template>
  <div class="card" id="whole-card">
  <div class="card-body">
    <h1 class="card-title">Tic Tac Toe</h1>
      <div class="card-body" id="set-up"> 
        <h5 style="text-align: center;"> Player 1 is O and Player 2 is X. First one to connect 3 in a row wins!</h5>
        <div class="card-body" id="grid">
          <Boxes />
      </div>
        
    </div>
  </div>
</div>
</template>

<script>

import Boxes from './Boxes.vue';
export default {
  name: 'Background',
  props: {
    msg: String
  },
  components: {
    Boxes
}, data() {
  return {
    player1: "Player 1",
    player2: "Player 2",
    numBoxes: 6,
    numRows: 2,
    numCols: 3
  }
}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #whole-card {
    width: 30%;
    margin-left: auto;
    margin-right: auto;
    margin-top: 5%;
    background-color:#78cc8e;
    border-radius: 2%;
  }
  .card-title {
    background-color: #c0e34d;
    text-align: center;
  }
  #set-up {
    background-color: #c0e34d;
  }
</style>
