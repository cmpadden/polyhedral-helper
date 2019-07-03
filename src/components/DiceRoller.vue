<template>

  <b-container fluid>

    <b-row>
      <b-col>
        <b-img center v-bind:src="dice_image" fluid alt="Dice"></b-img>
      </b-col>
      <b-col cols="10">
        <b-row>
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="inputGroup-sizing-sm"># Dice</span>
            </div>
            <input v-model="num_dice" type="text" class="form-control" placeholder="" aria-label="" aria-describedby="basic-addon1">
            <div class="input-group-append">
              <button v-on:click="num_dice += 1" class="btn btn-outline-secondary" type="button">+</button>
              <button v-on:click="num_dice -= 1" class="btn btn-outline-secondary" type="button">-</button>
            </div>
          </div>
        </b-row>

        <b-row>
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text"># Sides</span>
            </div>
            <b-form-select v-model="num_sides" :options="available_dice"></b-form-select>
          </div>
        </b-row>

        <b-row>
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="inputGroup-sizing-sm">Modifier</span>
            </div>
            <input v-model="mod_amount" type="text" class="form-control" placeholder="" aria-label="" aria-describedby="basic-addon1">
            <div class="input-group-append">
              <button v-on:click="mod_amount += 1" class="btn btn-outline-secondary" type="button">+</button>
              <button v-on:click="mod_amount -= 1" class="btn btn-outline-secondary" type="button">-</button>
            </div>
          </div>
        </b-row>
      </b-col>
    </b-row>

    <b-row>
      <b-col lg="1" class="pl-0">
        <b-button v-on:click="roll" variant="outline-primary">Roll</b-button>
      </b-col>
      <b-col lg="1" class="pl-0">
        <b-button v-on:click="clear" variant="outline-danger">Clear</b-button>
      </b-col>
    </b-row>

    <div v-if="roll_result.length">
      <hr />
      <!-- TODO: implement stats -->
      <!-- <div> -->
      <!-- Average: {{ stats.average }} -->
      <!-- </div> -->
      <b-table hover :items="roll_result"></b-table>
    </div>

</b-container>

</template>

<script>
export default {
  name: 'DiceRoller',
  props: {
    initial_num_dice: {
      type: Number,
      default: 1
    },
    initial_num_sides: {
      type: Number,
      default: 20
    },
    initial_mod_type: {
      type: String,
      default: '+'
    },
    initial_mod_amount: {
      type: Number,
      default: 0
    }
  },
  data () {
    return {
      num_dice: this.initial_num_dice,
      num_sides: this.initial_num_sides,
      mod_type: this.initial_mod_type,
      mod_amount: this.initial_mod_amount,
      roll_result: [],
      available_dice: [ 20, 12, 10, 8, 6, 4 ]
    }
  },
  methods: {
    roll: function (event) {
      for (var i = 0; i < this.num_dice; i++) {
        var base_value = Math.floor((Math.random() * this.num_sides) + 1)
        var mod_value = 0
        var roll_time = new Date()
        if (this.mod_type === '+') {
          mod_value = base_value + (this.mod_amount * 1)
        } else {
          mod_value = base_value - (this.mod_amount * 1)
        }

        // `unshift` to prepend value to array
        this.roll_result.unshift({
          roll: mod_value,
          base_roll: base_value,
          modifier: this.mod_amount,
          timestamp: roll_time.toLocaleTimeString(),
          _cellVariants:  base_value === this.num_sides ? { value: 'warning' } : {}
        })
      }
    },
    clear: function (event) {
      this.roll_result = []
    }
  },
  computed: {
    stats: function () {
      var sum = 0
      var length = this.roll_result.length
      for (var i = 0; i < length; i++) {
        sum += this.roll_result[i].value
      }
      return { average: sum / length }
    },
    dice_image: function () {
      return require('../assets/img/d' + this.num_sides + '.svg')
    }
  }
}
</script>

<style scoped lang="scss">

.btn-roll {
  width: 20rem;
}

</style>
