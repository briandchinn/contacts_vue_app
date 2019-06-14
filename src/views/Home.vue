<template>
  <div class="container">

    <h2>Create New Contact</h2>
    <div>
      First Name: <input type="text" v-model="newFirstName"><br>
      Middle Name: <input type="text" v-model="newMiddleName"><br>
      Last Name: <input type="text" v-model="newLastName"><br>
      Bio: <input type="text" v-model="newBio"><br>
      Email: <input type="text" v-model="newEmail"><br>
      Phone Number: <input type="text" v-model="newPhoneNumber"><br>
      User ID: <input type="number" v-model="newUserID"><br>
      <button v-on:click="createContact()">Create Contact</button>
    </div>

    <h1>All Contacts</h1>
    <div v-for="contact in contacts">
      <h2>{{ contact.first_name }} {{ contact.last_name }}</h2>
      <button v-on:click="showContact(contact)">Show More</button>
      <div v-if="currentContact === contact">
        Contact: {{ contact }}
        <p>{{ contact.bio}}</p>
        <p>{{ contact.email }}</p>
        <p>{{ contact.phone_number }}</p>
        <p>{{ contact.user_id }}</p>
        <div>
          First Name: <input type="text" v-model="contact.first_name"><br>
          Middle Name: <input type="text" v-model="contact.middle_name"><br>
          Last Name: <input type="text" v-model="contact.last_name"><br>
          Bio: <input type="text" v-model="contact.bio"><br>
          Email: <input type="text" v-model="contact.email"><br>
          Phone Number: <input type="text" v-model="contact.phone_number"><br>
          User ID: <input type="number" v-model="contact.user_id"><br>
          <button v-on:click="updateContact(contact)">Update Contact</button>
          <button v-on:click="destroyContact(contact)">Delete Contact</button>
        </div>
        <hr>
      </div>
    </div>



  </div>
</template>


<script>
// @ is an alias to /src
import axios from "axios";

export default {
  data: function(){
    return {
      contacts: [],
      currentContact: {},
      newFirstName: "",
      newMiddleName: "",
      newLastName: "",
      newBio: "",
      newEmail: "",
      newPhoneNumber: "",
      newUserID: ""
    };
  },
  created: function(){
    axios.get("api/contacts").then(response => {
      this.contacts = response.data;
    });
  },
  methods: {
    createContact: function(){
      var params = {
        first_name: this.newFirstName,
        middle_name: this.newMiddleName,
        last_name: this.newLastName,
        bio: this.newBio,
        email: this.newEmail,
        phone_number: this.newPhoneNumber,
        user_id: this.newUserID
      };
      axios.post("api/contacts", params).then(response => {
        console.log("Success!", response.data);
        this.contacts.push(response.data);
        this.newFirstName = "";
        this.newMiddleName = "";
        this.newLastName = "";
        this.newBio = "";
        this.newEmail = "";
        this.newPhoneNumber = "";
        this.newUserID = "";
      });
    },
    showContact: function(contact){
      if (this.currentContact === contact) {
        this.currentContact = {};
      } else {
        this.currentContact = contact;
      }
    },
    updateContact: function(contact){
      var params = {
        first_name: contact.first_name,
        middle_name: contact.middle_name,
        last_name: contact.last_name,
        bio: contact.bio,
        email: contact.email,
        phone_number: contact.phone_number,
        user_id: contact.user_id
      };
      axios.patch("/api/contacts/" + contact.id, params).then(response => {
      console.log("Success!", response.data);
      contact = response.data;
      });
    },
    destroyContact: function(contact){
      axios.delete("api/contacts/" + contact.id).then(response => {
        console.log("Success", response.data);
        var index = this.contacts.indexOf(contact);
        this.contacts.splice(index, 1);
      });
    }
  }
};
</script>
