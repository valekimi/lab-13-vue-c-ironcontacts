<script setup>
import contacts from "./contacts.json";
import { ref } from "vue";

const contactsList = ref(contacts.slice(0, 5));

const addRandomContact = () => {
  const remainingContacts = contacts.filter(
    (contact) => !contactsList.value.includes(contact)
  );
  if (remainingContacts.length > 0) {
    const randomIndex = Math.floor(Math.random() * remainingContacts.length);
    const randomContact = remainingContacts[randomIndex];
    contactsList.value.push(randomContact);
  } else {
    console.log("All contacts have been displayed");
  }
};

const sortByName = () => {
  contactsList.value.sort((a, b) => a.name.localeCompare(b.name));
};

const sortByPopularity = () => {
  contactsList.value.sort((a, b) => b.popularity - a.popularity);
};

const removeContact = (id) => {
  const index = contactsList.value.findIndex((contact) => contact.id === id);
  if (index !== -1) {
    contactsList.value.splice(index, 1);
    contactsList.value = [...contactsList.value]; // Update the ref to trigger reactivity
  }
};
</script>

<template>
  <h1>Iron Contacts</h1>
  <div>
    <button @click="addRandomContact">Add Random Contact</button>
    <button @click="sortByName">Sort by Name</button>
    <button @click="sortByPopularity">Sort by Popularity</button>
  </div>
  <br />
  <br />
  <table>
    <thead>
      <tr>
        <th>Picture</th>
        <th>Name</th>
        <th>Popularity</th>
        <th>Won Oscar</th>
        <th>Won Emmy</th>
        <th>Action</th>
      </tr>
    </thead>
    <br>
    <tbody>
      <tr v-for="contact in contactsList" :key="contact.id">
        <td><img :src="contact.pictureUrl" alt="Contact Picture" /></td>
        <td>{{ contact.name }}</td>
        <td>{{ contact.popularity }}</td>
        <td>
          <span v-if="contact.wonOscar">🏆</span>
        </td>
        <td>
          <span v-if="contact.wonEmmy">🏆</span>
        </td>
        <td><button @click="removeContact(contact.id)">Delete</button></td>
      </tr>
    </tbody>
  </table>
</template>

<style>
img {
  width: 80px;
  height: auto;
}

tr {
  display: flex;
}

th {
  width: 120px;
  display: flex;
  align-content: center;
}

td {
  width: 120px;
}

button {
  padding: 8px 16px;
  border: none;
  background-color: #ffff00;
  color: black;
  border-radius: 4px;
}

</style>
