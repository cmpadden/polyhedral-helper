<template>
  <div class="home">
    <DiceRoller/>
  </div>
</template>

<script>
// @ is an alias to /src
import DiceRoller from '@/components/DiceRoller.vue'

export default {
  name: 'home',
  components: {
    DiceRoller
  }
}
</script>
