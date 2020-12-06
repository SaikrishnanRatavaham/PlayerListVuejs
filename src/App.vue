<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <PlayerList />
  </div>
</template>

<script>
import PlayerList from './components/PlayerList.vue'

export default {
  name: 'App',
  components: {
    PlayerList
  }
}
</script>

<style>



.card {
       border: initial;
    background: #fff;
    border-radius: .1rem;
    border: 0px solid rgba(0, 0, 0, 0.125);
    box-shadow: 0 1px 15px rgba(0,0,0,.04), 0 1px 6px rgba(0,0,0,.04);
}
.rounded .card {
    border-radius: .75rem;
}
.card .card-body {
    padding: 1.75rem;
}
</style>
