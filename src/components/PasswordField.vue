<template>
  <div class="container"> 
    <input :type="show === true ? 'text' : 'password'" id="password" v-model="password">
    <div class="strength" :class="'level_' + strengthLevel"></div>

    <i class="fas fa-eye show-icon" v-if="show" @click="show = !show"></i>
    <i class="fas fa-eye-slash hide-icon" v-else @click="show = !show"></i>
  
    <i class="fas fa-frown level-icon" v-if="strengthLevel === 1"></i>
    <i class="fas fa-meh level-icon" v-if="strengthLevel === 2"></i>
    <i class="fas fa-smile level-icon" v-if="strengthLevel === 3"></i>
    <i class="fas fa-grin-stars level-icon" v-if="strengthLevel === 4"></i>
  </div>
</template>

<script>
  export default {
    data: () => {
      return {
        password: '',
        show: false
      }
    },
    computed: {
      scorePassword() {
        let score = 0;
        if(this.password === '') return score;

        let letters = {};
        for(let i = 0; i < this.password.length; i++) {
          letters[this.password[i]] = (letters[this.password[i]] || 0) + 1;
          score += 5.0 / letters[this.password[i]];
        }

        let variations = {
          digits: /\d/.test(this.password),
          lower: /[a-z]/.test(this.password),
          upper: /[A-Z]/.test(this.password),
          special: /\W/.test(this.password)
        };

        let variationsCount = 0;
        for (let check in variations) {
          variationsCount += (variations[check] === true) ? 1 : 0;
        }

        score += (variationsCount - 1) * 10;

        return parseInt(score);
      },
      strengthLevel() {
        let pass = this.scorePassword;
        if(pass === 0) return 0;
        if(pass < 25) return 1;
        if(pass < 50) return 2;
        if(pass < 75) return 3;
        if(pass >= 75) return 4;
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import url('https://use.fontawesome.com/releases/v5.7.2/css/all.css');

  .container {
    position: relative;
    display: flex;
    justify-content: center;
    width: 500px;
  }

  input {
    position: relative;
    color: #333;
    width: calc(100% - 140px);
    font-size: 3rem;
    padding: 6px 70px 4px;
    border: none;
    border-radius: 40px;
    overflow: hidden;
    z-index: 10;

    &:focus {
      outline: none;
    }
  }

  .strength {
    position: absolute;
    bottom: -10px;
    left: 0;
    right: 0;
    display: block;
    width: 25%;
    height: 100%;
    background-color: #a5df41;
    border-radius: 40px;
    overflow: hidden;
    z-index: 9;
    transition: all .5s linear;
  }

  @mixin level($position, $width, $color) {
    bottom: $position;
    width: $width;
    background-color: $color;
  }

  .level_0 {
    @include level(0, 25%, #BB4440);
  }
  .level_1 {
    @include level(-10px, 25%, #BB4440);
  }
  .level_2 {
    @include level(-10px, 50%, #E17D30);
  }
  .level_3 {
    @include level(-10px, 75%, #F0B03F);
  }
  .level_4 {
    @include level(-10px, 100%, #a5df41);
  }

  .level-icon {
    position: absolute;
    right: 20px;
    top:20px;
    z-index: 11;
  }

  @mixin showpass($property) {
    position: absolute;
    left: $property;
    top: 20px;
    z-index: 11;
    cursor: pointer;
  }

  .show-icon {
    @include showpass(20px);
  }

  .hide-icon {
    @include showpass(18px);
    color: #555;
  }
</style>
