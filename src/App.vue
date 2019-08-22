<template>
  <div id="app">
    <Flights />
  </div>
</template>

<script>
import Flights from './components/Flights';

export default {
  name: 'app',
  components: {
    Flights
  }
}
</script>

<style>
  body {
    background: #ecfbff !important;
    font-size: 14px !important;
    color: #212C5B !important;
    padding-top: 100px;
    padding-bottom: 100px;
  }

  table {
    color: #212C5B !important;
  }
</style>
