<template>
  <div class="hello">
      <h1> Hey there, Welcome to Place Finder </h1>
      <pre> <i> Feel free to search for a place </i> </pre>
    <button class="button is-info" @click="showplaces = !showplaces"> View Our places </button> <br>
    <detect-network> 
        <div slot="offline"> <pre> :-( You are offline => Our places are only visible online.  </pre> </div>
        <div slot="online">
            <vue-good-table
            v-show="showplaces"
            :columns="columns"
            :rows="places"
              :search-options="{
                enabled: true,
                placeholder: 'Make Search',
            }"
            />
        </div>
    </detect-network>
</div>
</template>

<script>
import secretURL from "./secretURL.js";
import detectNetwork from "v-offline";
import { VueGoodTable } from "vue-good-table";
import "vue-good-table/dist/vue-good-table.css";

export default {
  props: {
    msg: String
  },
  components: {
    "detect-network": detectNetwork,
    VueGoodTable
  },
  data() {
    return {
      columns: [
        {
          label: 'Name',
          field: 'name'
        },
        {
          label: 'Location',
          field: 'location'
        },
      ],
      places: [],
      showplaces: false
    };
  },
  mounted() {
    let url = secretURL;
    fetch(url)
      .then(resp => resp.json()) // Transform the data into json
      .then(data => {
        let results = data["features"];
        results.forEach(element => {
          this.places.push({
            id: element.id,
            name: element.properties.name,
            location: `${element.properties.lga_name}, ${element.properties.state_name}`
          });
        });
      });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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
