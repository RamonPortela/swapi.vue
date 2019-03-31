<template>
  <div>
    <ul>
      <li>{{planet.name}}</li>
      <li>{{planet.climate}}</li>
      <li>{{planet.population}}</li>
      <li>{{planet.terrain}}</li>
      <li>{{planet.filmTitles.join(',')}}</li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    planet: {
               type: Object,
               required: true,
               default: () => {},
            }
  }
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
