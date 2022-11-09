<template>
    <form @submit.prevent="addItem" id="itemAdd" class="d-flex">
      <input v-model="item" required type="text" placeholder="Přidat položku" class="form-control" width="80%" />
      <button type="submit" class="btn btn-primary">Přidat položku</button>
    </form>
    <p v-if="error.length > 0" class="text-danger">{{error}}</p>
    <p v-if="items.length === 0" class="text-secondary mt-3">Seznam je prázdný.</p>
    <table class="table table-stripped table-hover mt-3">
      <tbody>
        <tr v-bind:key="item" v-for="(item, index) in items">
          <td>{{item}}</td>
          <td><button class="btn btn-danger" @click="removeItem(index)">Smazat</button></td>
        </tr>
      </tbody>
    </table>
  </template>
  
  <script>
    export default {
      name: "TodoComponent",
      data(){
        return{
          items: localStorage.itemlist !== null ? JSON.parse(localStorage.itemlist) : [],
          item: '',
          error: ''
        }
      },
      methods:{
        addItem(){
          if(this.items.includes(this.item) === true){
            this.error = "Tento úkol v seznamu již je.";
            return;
          }
          this.error = "";
          this.items.push(this.item);
          localStorage.itemlist = JSON.stringify(this.items);
        },
        removeItem(index){
          this.items.splice(index, 1);
          localStorage.itemlist = JSON.stringify(this.items);
        }
      }
    }
  </script>
  