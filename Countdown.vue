<template>
<ul class="vue-countdown">
    <li>
        <p class="digit">{{ days | twoDigits }}</p>
        <p class="text">days</p>
    </li>

    <li>
        <p class="digit">{{ hours | twoDigits }}</p>
        <p class="text">hours</p>
    </li>

    <li>
        <p class="digit">{{ minutes | twoDigits }}</p>
        <p class="text">Min</p>
    </li>

    <li>
        <p class="digit">{{ seconds | twoDigits }}</p>
        <p class="text">Sec</p>
    </li>
</ul>
</template>

<script>
import Vue from 'vue'

Vue.filter('twoDigits', (value) => {
    if ( value.toString().length <= 1 ) {
        return '0'+value.toString()
    }
    return value.toString()
})

export default {
    props: ['deadline'],

    data() {
        return {
            now: Math.trunc((new Date()).getTime() / 1000),
            date: null
        }
    },

    mounted() {
        this.date = Math.trunc(Date.parse(this.deadline) / 1000)

        setInterval(() => {
            this.now = Math.trunc((new Date()).getTime() / 1000)
        }, 1000)
    },

    computed: {
        seconds() {
            return Math.trunc(this.date - this.now) % 60
        },

        minutes() {
            return Math.trunc((this.date - this.now) / 60) % 60
        },

        hours() {
            return Math.trunc((this.date - this.now) / 60 / 60) % 24
        },

        days() {
            return Math.trunc((this.date - this.now) / 60 / 60 / 24)
        }
    }
}
</script>
<style lang="sass" rel="stylesheet/sass">
.vue-countdown
  padding: 0
  margin: 0

  li
    display: inline-block
    margin: 0 8px
    text-align: center
    position: relative

    &:after
        content: ':'
        position: absolute
        top: 0
        right: -13px
        font-size: 32px

    &:first-of-type
      margin-left: 0

    &:last-of-type
      margin-right: 0

      &:after
        content: ''

  .digit
      font-size: 32px
      font-weight: 600
      line-height: 1.4
      margin-bottom: 0

  .text
      text-transform: uppercase
      margin-bottom: 0
      font-size: 10px

</style>
