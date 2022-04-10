<script>

import contacts from "./contacts.json";

export default {
  data() {
    return {
      contacts: [],
      orderCriteria: ''
    }
  },
  created() {
    // Esto es un gusto personal. Me gusta transformar y/o cargar los datos en este hook, pero no veo ninguna razón para no hacerlo directamente en data(). Quizás, como aquí he tenido que poner bastante código, considero que queda más claro hacerlo aquí.

    // Estrategia sugerida por Juan Pablo: transformamos los datos añadiendo un campo nuevo para indicar cuando un contacto ha de ser visible o no, en vez de mantener dos arrays o hacer otros tipos de complicaciones. Reescribir los datos de entrada con un formato que nos sea más fácil de manejar es una técnica ampliamente utilizada.

    // Añadimos un campo nuevo .isVisible; que nos indicará si debemos mostrar el contacto o no. Solo vamos a mostrar los 5 primeros.

    this.contacts = contacts.map((contact, index) => {
      return {
        ...contact,
        isVisible: index <= 4
      }
    });
  },
  methods: {
    addRandomContact() {
      // Random entre 0 y todos los contactos que quedan por mostrar (campo isVisible es false)
      const hiddenContacts = this.contacts.filter(contact => !contact.isVisible)
      const index = Math.floor(Math.random() * hiddenContacts.length);
      // Ahora dicho contacto debe ser visible
      this.contacts[index].isVisible = true;
    },
    removeContact(contact) {
      this.contacts = this.contacts.filter(c => c.id != contact.id);
    }
  },
  computed: {
    getContacts() {

      if (this.orderCriteria == 'name') {
        this.contacts.sort((c1, c2) => {
          return (c1.name > c2.name) ? 1 : -1;
        })
      }

      else if (this.orderCriteria == 'popularity') {
        this.contacts.sort((c1, c2) => {
          return c2.popularity - c1.popularity;
        })
      }
      return this.contacts.filter(contact => contact.isVisible);
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
      <button @click="orderCriteria = 'name'">Sort By Name</button>
      <button @click="orderCriteria = 'popularity'">Sort By Popularity</button>
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
        <tr v-for="contact in getContacts" :key="contact.id">
          <td>
            <img class="photo" :src="contact.pictureUrl" />
          </td>
          <td>{{ contact.name }}</td>
          <td>{{ contact.popularity.toFixed(2) }}</td>
          <td><button @click="removeContact(contact)">Remove</button></td>
          <td>
            <img v-if="contact.wonOscar" src="https://github.githubassets.com/images/icons/emoji/unicode/1f3c6.png"
              alt="oscar trophy" />
          </td>
          <td>
            <img v-if="contact.wonEmmy" src="https://github.githubassets.com/images/icons/emoji/unicode/1f3c6.png"
              alt="emmy trophy" />
          </td>
        </tr>
      </tbody>
    </table>  </div>
</template>

<!-- estilos globales de la aplicación -->
<style>#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.photo {
  width: 100px;
}</style>
