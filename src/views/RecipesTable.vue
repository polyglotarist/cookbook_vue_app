<template>
  <div class="recipes-index">
    <h1>All Recipes</h1>
    <div>
      Filter Title: <input v-model="titleFilter" list="titles">

      <datalist id="titles">
        <option v-for="recipe in recipes">{{ recipe.title }}</option>
      </datalist>
    </div>

    <div>
      <button v-on:click="setSortAttribute('title')">Sort by Title</button>
      <button v-on:click="setSortAttribute('prep_time')">Sort by Prep Time</button>
    </div>

    <table style="width:100%">
      <tr>
        <th>ID</th>
        <th>Title</th> 
        <th>Chef</th>
        <th>Prep Time</th>
      </tr>
      <tr v-for="recipe in orderBy(filterBy(recipes, titleFilter, 'title'), sortAttribute)">
        <td>
          {{ recipe.id }}
        </td>
        <td>
          <router-link v-bind:to="'/recipes/' + recipe.id">
            {{ recipe.title }}
          </router-link>
        </td>
        <td>
          {{ recipe.chef }}
        </td>
        <td>
          {{ recipe.formatted.prep_time }}
        </td>
      </tr>
    </table>

  </div>
</template>

<style>
</style>

<script>
var axios = require('axios');
import Vue2Filters from "vue2-filters";

export default {
  data: function() {
    return {
      recipes: [],
      currentRecipe: {},
      titleFilter: '',
      sortAttribute: 'title'
    };
  },
  created: function() {
    axios.get("/api/recipes")
      .then(response => {
        this.recipes = response.data;
      });
  },
  methods: {
    setSortAttribute: function(inputAttribute) {
      this.sortAttribute = inputAttribute;
    }
  },
  mixins: [Vue2Filters.mixin]
};
</script>














