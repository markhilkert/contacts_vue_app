<template>
  <div class="home">
  <h1>New Contact</h1>
    <div>
      First Name: <input type="text" v-model="newContactFirstName">
      Middle Name: <input type="text" v-model="newContactMiddleName">
      Last Name: <input type="text" v-model="newContactLastName">
      Email: <input type="text" v-model="newContactEmail">
      Phone: <input type="text" v-model="newContactPhoneNumber">
      Bio: <input type="text" v-model="newContactBio">
      <button v-on:click="createContact()">Create Contact</button>
    </div>

    <h1>All Contacts</h1>
    <div v-for="contact in contacts">
      <h2>{{ contact.formatted.full_name }}</h2>
      <button v-on:click="showContact(contact)">Show More</button>
      <div v-if="currentContact === contact">
        <p>E-mail: {{ contact.email }}</p>
        <p>Phone Number: {{ contact.phone_number }}</p>
        <p>Bio: {{ contact.bio }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      contacts: [],
      currentContact: {},
      newContactFirstName: "",
      newContactMiddleName: "",
      newContactLastName: "",
      newContactEmail: "",
      newContactPhoneNumber: "",
      newContactBio: ""
    };
  },
  created: function() {
    axios.get("/api/contacts").then(response => {
      this.contacts = response.data;
    });
  },
  methods: {
    createContact: function() {
      console.log("Creating a new contact...")
      var params = {
                    first_name: this.newContactFirstName,
                    middle_name: this.newContactMiddleName,
                    last_name: this.newContactLastName,
                    email: this.newContactEmail,
                    phone_number: this.newContactPhoneNumber,
                    bio: this.newContactBio
                    };
      axios.post("api/contacts", params)
        .then( response => {
          this.contacts.push(response.data);
          this.newContactFirstName = "";
          this.newContactMiddleName = "";
          this.newContactLastName = "";
          this.newContactEmail = "";
          this.newContactPhoneNumber = "";
          this.newContactBio = "";
        });
    },
    showContact: function(contact) {
      if (this.currentContact === contact) {
        this.currentContact = {}; 
      } else {
        this.currentContact = contact;
      }
    }
  }
};
</script>