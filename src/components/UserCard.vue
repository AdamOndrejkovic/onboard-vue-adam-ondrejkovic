<template>
<div class="userCard" v-bind:class="{selected: selected}" v-on:click="changeSelection">
  <div class="image">
    <img src="src/assets/images/profile.svg" alt="Image">
  </div>
  <div class="main text-center">
    <ul class="list-none">
      <li>{{name}}</li>
      <li>{{email}}</li>
      <li>{{company}}</li>
      <li>{{phone}}</li>
    </ul>
  </div>
</div>
</template>

<script>
export default {
  props:['name', 'company', 'email', 'phone'],
  name: "UserCard",
  data: function() {
    return {
      selected: false
    }
  },
  methods: {
    changeSelection: function () {
      this.selected =  !this.selected;
      this.$emit('changeSelectionCounter', this.selected);
    }
  }
}
</script>

<style scoped>
.userCard{
  padding: 15px 10px;
  border: 2px cornflowerblue solid;
  border-radius: 25px;
  width: 300px;
  margin: auto;
}

.selected {
  border: 2px green solid;
}

.image img {
  width: 125px;
  margin: auto;
}

.main {
  padding: 15px 5px 5px;
}
</style>
