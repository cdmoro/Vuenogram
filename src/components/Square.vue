<template>
  <div :class="['col cell', {filled: clicked}]" @click="onClick">&nbsp;</div>
</template>

<script>
export default {
  name: 'Square',
  props: {
    coords: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      clicked: false
    }
  },
  methods: {
    onClick() {
      this.clicked = !this.clicked
      this.$emit('clickedSquare', this.clicked, this.coords)
    }
  }
}
</script>

<style scoped>
  .cell {
    cursor: pointer;
  }

  .filled {
	  background-color: #41B883;
	  border: #41B883 solid 1px;
  }
</style>
