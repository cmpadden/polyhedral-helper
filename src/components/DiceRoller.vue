<template>

  <b-container fluid>

    <b-row>
      <b-col sm="12" md="2" class="text-center align-self-center" >
          <b-img v-bind:src="dice_image" fluid alt="Die"></b-img>
      </b-col>
      <b-col sm="12" md="10">
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
      <b-col xs="6" class="text-center">
        <b-button block v-on:click="roll" variant="outline-primary">Roll</b-button>
      </b-col>
      <b-col xs="6" class="text-center">
        <b-button block v-on:click="clear" variant="outline-danger">Clear</b-button>
      </b-col>
    </b-row>

    <div v-if="roll_result.length">
      <hr />
      <!-- TODO: implement stats -->
      <!-- <div>Average: {{ stats.average }} </div> -->
      <b-table responsive hover :items="roll_result">

        <template slot="roll_components" slot-scope="row">
          <b-card>
            <b-row class="mb-1" v-for="(value, key) in row.value" :key="key">
              <b-col class="text-sm-right"><b>Roll #{{ value.iter }}:</b></b-col>
              <b-col v-bind:class="{ 'text-success': value.max }">{{ value.base_roll }}</b-col>
            </b-row>
          </b-card>
        </template>

      </b-table>
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

      // Make previous rolls secondary
      for (var i = 0; i < this.roll_result.length; i++) {
        this.roll_result[i]._rowVariant = 'secondary'
      }

      // Sum rolls and keep track of individual rolls in `roll_components`
      var roll_total = 0
      var roll_components = []
      for (var d = 0; d < this.num_dice; d++) {
        var roll = Math.floor((Math.random() * this.num_sides) + 1)
        roll_total += roll
        roll_components.push({
          iter: d + 1,
          base_roll: roll,
          max: this.num_sides === roll
        })
      }

      // Add modifier to summed base roll
      roll_total += this.mod_amount
      var roll_max = (this.num_sides * this.num_dice) + this.mod_amount

      // `unshift` to prepend value to array
      this.roll_result.unshift({
        result: roll_total,
        roll_notation: '(' + this.num_dice + 'd' + this.num_sides + ')+' + this.mod_amount,
        roll_components: roll_components,
        _rowVariant:  roll_total === roll_max ? 'warning' : null
      })

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

button {
  /* Disable double-tap to zoom */
  touch-action: manipulation;
}

</style>
