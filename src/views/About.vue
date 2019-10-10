<template>
  <div class="about flex flex-col items-center">
      <div class="absolute inset-0 z-0" @click="modal=false"></div>
    <input type="text" class="bg-200; border-4 px-4 py-2 mb-1 z-10 mt-1" @focus="modal=true" autocomplete="off" v-model="state">
      <div v-if="filteredStates && modal" class="z-10">
          <ul class="w-48 bg-gray-800 text-white">
              <li class="py-2 border-b cursor-wait" v-for="filteredState in filteredStates" @click="setState(filteredState)">{{filteredState}}</li>
          </ul>
      </div>
      <!--<div v-for="user in users" :key="user.id">{{user.name}}</div>-->
  </div>
</template>

<script>
    window.axios = require('axios');

  export default{
      name: 'About',
      data: function () {
          return{
              users: null,
              state: '',
              modal: false,
              states: [
                  'Alabama', 'Florida', 'Texas', 'Alaska', 'Philly', 'Washington', 'oaklohama', 'Yorkshire'
              ],
              filteredStates: [],
          }
      },
      mounted(){
          axios.get('https://jsonplaceholder.typicode.com/users').then(response => this.users = response.data).catch(response => this.users = [{name: 'No users found'}]).finally(console.log('Data loading completed'));
          this.filterStates();
      },
      methods: {
          filterStates(){
              for(var i = 0; i <= this.states.length; i++){
                  if(this.state.length == 0 || null){
                      this.filteredStates = this.states;
                  }
                  this.filteredStates = this.states.filter(state => {
                      return state.toLowerCase().startsWith(this.state.toLowerCase());
                  });
              }
          },
          setState(state){
              this.state = state;
              this.modal = false;
          }
      },
      watch: {
          state(){
              this.filterStates();
          }
      }
  }
</script>