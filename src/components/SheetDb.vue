<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>Base de donnée Google Sheet :</h2>
    <h3>
      <a href="https://docs.google.com/spreadsheets/d/1MvB57bAcJrH157Gmt5_0DSqDfEXkcKSuXHj4NWzz4io/edit?usp=sharing" target="_blank">
        Voici la base de donnée
      </a>
    </h3>
    <ul>
      <li v-for="person in p" :key="p.id">
        {{person.Firstname}} {{person.Lastname}}
        <button class="deletebutton" type="button" name="button" v-on:click="deletePerson(person)">Supprimer</button>
      </li>
    </ul>
    <input type="text" name="AddFirstname" value="" placeholder="Prénom" v-model="firstName">
    <input type="text" name="AddLastname" value="" placeholder="Nom" v-model="lastName">
    <button class="addbutton" type="button" name="button" v-on:click="addPerson">Ajouter une personne</button>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'SheetDb',
  data () {
    return {
      p: {},
      msg: 'Vuejs + Sheet DB',
      id: '',
      firstName: '',
      lastName: ''
    }
  },
  mounted () {
    let self = this

    axios.get('https://sheetdb.io/api/v1/5a6eede72fd41').then((response) => {
      self.p = response.data
    })

    axios.get('https://sheetdb.io/api/v1/5a6eede72fd41/count').then((response) => {
      self.id = response.data.rows
    })

  },
  methods: {
    addPerson () {
      this.id += 1

      let person = {
        id: this.id,
        Firstname: this.firstName,
        Lastname: this.lastName
      }

      let savePerson = {
        data: [
          person
        ]
      }

      if (person.Firstname && person.Lastname) {
        axios.post('https://sheetdb.io/api/v1/5a6eede72fd41', savePerson)

        this.p.push(person)

        this.firstName = ''
        this.lastName = ''

      } else {
        alert('Le Nom et le Prénom sont requis !')
      }
    },
    deletePerson (person) {
      let personIndex = this.p.indexOf(person)

      axios.delete('https://sheetdb.io/api/v1/5a6eede72fd41/id/' + person.id )

      this.p.splice(personIndex, 1)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: list-item;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.addbutton {
  outline: none;
  background-color: white;
  color: black;
  border: 1px solid #4CAF50;
  padding: 4px 30px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 15px;
  margin: 4px 2px;
  -webkit-transition-duration: 0.4s; /* Safari */
  transition-duration: 0.4s;
  cursor: pointer;
}
.addbutton:hover {
    background-color: #4CAF50;
    color: white;
}
.deletebutton {
  outline: none;
  background-color: white;
  color: black;
  border: 1px solid #f44336;
  padding: 4px 30px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 15px;
  margin: 4px 2px;
  -webkit-transition-duration: 0.4s; /* Safari */
  transition-duration: 0.4s;
  cursor: pointer;
}
.deletebutton:hover {
    background-color: #f44336;
    color: white;
}
</style>
