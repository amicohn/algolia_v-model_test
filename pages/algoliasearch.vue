<template>
  <ais-instant-search index-name="demo_ecommerce" :search-client="searchClient">
    
    <!-- facetQuery ???? -->
    <ais-configure :hitsPerPage="8" :query="search_query" :facetQuery="brand_query"/>

    <div class="left-panel">
      <h2>Brands</h2>

      <ais-refinement-list attribute="brand" searchable show-more>
        <div
          slot-scope="{
      items,
      isShowingMore,
      isFromSearch,
      canToggleShowMore,
      refine,
      createURL,
      toggleShowMore,
      searchForItems
    }"
        >
          <v-text-field
            v-model="brand_query"
            type="search"
            placeholder="I want to set up here with v-model, but, not working"
          />

          <input
            @input="searchForItems($event.currentTarget.value)"
            placeholder="Algolia original form.Here is working."
            style="border: solid medium lime; height:50px; width:350px;"
          >

          <v-list>
            <span v-if="isFromSearch && !items.length" style="color:red">No results.</span>
            <li v-for="item in items" :key="item.value">
              <v-chip
                :href="createURL(item)"
                :style="{ backgroundColor: item.isRefined ?  'red' : '' }"
                @click.prevent="refine(item.value)"
              >
                <ais-highlight attribute="item" :hit="item"/>
                ({{ item.count.toLocaleString() }})
              </v-chip>
            </li>
          </v-list>
          <button
            @click="toggleShowMore"
            :disabled="!canToggleShowMore"
          >{{ !isShowingMore ? 'Show more' : 'Show less'}}</button>
        </div>
      </ais-refinement-list>
    </div>
    <div class="right-panel">
      <ais-search-box>
        <v-text-field
          name="name"
          label="Search"
          v-model="search_query"
          placeholder="Search / Enter Keywords"
        />
      </ais-search-box>
      <ais-hits>
        <div slot="item" slot-scope="{ item }">
          <h3>{{ item.name }}</h3>
          <img :src="item.image" align="left" :alt="item.name">
          <div class="hit-name">
            <ais-highlight attribute="name" :hit="item"></ais-highlight>
          </div>

          <div class="hit-description">
            <ais-highlight attribute="description" :hit="item"></ais-highlight>
          </div>
          <div class="hit-price">{{ item.price }}</div>
        </div>
      </ais-hits>
      <ais-pagination></ais-pagination>
    </div>
  </ais-instant-search>
</template>

<script>
import algoliasearch from "algoliasearch/lite";
import "instantsearch.css/themes/algolia-min.css";
export default {
  data() {
    return {
      searchClient: algoliasearch(
        "B1G2GM9NG0",
        "aadef574be1f9252bb48d4ea09b5cfe5"
      ),
      search_query: "",

      brand_query: ""
    };
  }
};
</script>


<style scoped>
body {
  font-family: sans-serif;
  padding: 1em;
}

.ais-Hits-list {
  margin-top: 0;
  margin-bottom: 1em;
}

.ais-InstantSearch {
  display: grid;
  grid-template-columns: 1fr 2fr;
  grid-gap: 1em;
}

.ais-Hits-item img {
  margin-right: 1em;
}
.hit-name {
  margin-bottom: 0.5em;
}
.hit-description {
  color: #888;
  font-size: 0.8em;
  margin-bottom: 0.5em;
}

.v-input--selection-controls {
  margin-top: 0px;
  padding-top: 4px;
}
</style>