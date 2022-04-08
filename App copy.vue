<script>

import contacts from "./contacts.json";

export default {
  data() {
    return {
      hiddenContacts: contacts.slice(5),
      visibleContacts: contacts.slice(0, 5)
    }
  },
  methods: {
    addRandomContact() {
      const index = Math.floor(Math.random() * this.hiddenContacts.length);
      console.log(index);
      this.visibleContacts.push(this.hiddenContacts[index]);
      this.hiddenContacts.splice(index, 1);
    }
  }
}
</script>

<!-- HTML de la APP-->
<template>
  <div id="app">
    <h1>SinguContacts</h1>
    <div class="container">
      <button @click="addRandomContact">Get Random Contact</button>
    </div>
    <table>
      <thead>
        <tr>
          <th>Picture</th>
          <th>Name</th>
          <th>Popularity</th>
          <th>Won Oscar</th>
          <th>Won Emy</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="contact in visibleContacts" :key="contact.id">
          <td>
            <img class="photo" :src="contact.pictureUrl" />
          </td>
          <td>{{ contact.name }}</td>
          <td>{{ contact.popularity.toFixed(2) }}</td>
          <td>
            <img
              v-if="contact.wonOscar"
              src="https://github.githubassets.com/images/icons/emoji/unicode/1f3c6.png"
              alt="oscar trophy"
            />
          </td>
          <td>
            <img
              v-if="contact.wonEmmy"
              src="https://github.githubassets.com/images/icons/emoji/unicode/1f3c6.png"
              alt="emmy trophy"
            />
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<!-- estilos globales de la aplicación -->
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.photo {
  width: 100px;
}
</style>
