<template>
    <Page>
        <ActionBar title="Pokemon WebService"/>
        <GridLayout columns="*" rows="*">
            <ListView for="p in pokemon" class="list-group">
              <v-template>
                <StackLayout class="list-group-item">
                  <label :text="p.name"  />
                </StackLayout>
              </v-template>
            </ListView>
        </GridLayout>
    </Page>
</template>

<script >

import * as http from "http";

  export default {
    data() {
      return {
        pokemon:[]
		  }
    },
    mounted(){
      http.getJSON("https://pokeapi.co/api/v2/pokemon/?limit=151")
      .then(result => {
        this.pokemon = result.results;
      }, error => {
        console.log(error);
      });
     }

    };
</script>

<style scoped>
    ActionBar {
        background-color: #53ba82;
        color: #ffffff;
    }

    .message {
        vertical-align: center;
        text-align: center;
        font-size: 20;
        color: #333333;
    }
</style>
