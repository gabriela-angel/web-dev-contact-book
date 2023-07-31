<script>
import contacts from "./utils/contacts"
import ContactList from "./components/ContactList.vue"
import ContactInfo from "./components/ContactInfo.vue"
import ContactForm from "./components/ContactForm.vue"
import EditForm from "./components/EditForm.vue"


export default {
  name:'App',

  components: {ContactList, 
              ContactInfo,
              ContactForm,
              EditForm
  },

  data: function () {
    return {
      contacts: contacts,
      filteredList: [],
      newContact: {
        id: null,
        lastName: '',
        firstName: '',
        email: ''
      },
      search: '',
      selectedContact: null,
      editedContact: null,
    }
  },

  methods: {
    addContact() {
      // alert(' its supposed to b adding')

      // Convert all string properties to lowercase
      for (const prop in this.newContact) {
        if (typeof this.newContact[prop] === 'string') {
          this.newContact[prop] = this.newContact[prop].toLowerCase();
        }
      }

      const newId = this.contacts.length +1
      this.newContact.id = newId

      this.contacts.push(this.newContact)
      
      this.newContact = {
        id: null,
        lastName: '',
        firstName: '',
        email: ''
      }

    },

    deleteContact (deletedContact) {
      // alert(' its supposed to b deleting')

      this.contacts = this.contacts.filter((contact) => contact.id !== deletedContact.id);
      this.selectedContact = null;
      this.editedContact = null;
    },

    editContact() {
      // alert(' its supposed to b editing')

      this.editedContact.firstName = this.editedContact.firstName.toLowerCase();
      this.editedContact.lastName = this.editedContact.lastName.toLowerCase();
      this.editedContact.email = this.editedContact.email.toLowerCase();

      const index = this.contacts.findIndex(contact => contact.id === this.editedContact.id);
      if (index !== -1) {
        this.contacts.splice(index, 1, this.editedContact);
      }

      this.editedContact = null;
    },

    openEdit(contact) {
      // alert('working')
      this.selectedContact = null;
      this.editedContact = contact;
    },

    openInfo(contact) {
      // alert(`working ${contact.firstName}`);
      this.selectedContact = contact;
      this.editedContact = null;
    },

    filter() {
      // alert(`youd search ${this.search}`)

      if (this.search.trim() === '') {
        this.filteredList = this.contacts
      } else {
        this.filteredList = this.contacts.filter((contact) => {
        const firstNameMatch = contact.firstName.toLowerCase().includes(this.search.toLowerCase());
        const lastNameMatch = contact.lastName.toLowerCase().includes(this.search.toLowerCase());
        return firstNameMatch || lastNameMatch;
        })
      }
    }
  },

  watch: {
    contacts: {
      handler: function(newContacts) {
        localStorage.setItem('contacts', JSON.stringify(newContacts));
        this.filteredList = newContacts;
      },
      deep: true
    }
  },

  created() {
    const storedItems = localStorage.getItem('contacts');
    if (storedItems) {
      this.contacts = JSON.parse(storedItems);
      this.contacts.sort((a, b) => a.lastName.toLowerCase() < b.lastName.toLowerCase() ? -1 : a.lastName.toLowerCase() > b.lastName.toLowerCase() ? 1 : 0);
      this.filteredList = this.contacts;
    } else {
      localStorage.setItem('contacts', JSON.stringify(contacts));
    }
    
  }
}
</script>

<template>
  <div>
    <h1>Contact Book</h1>
  </div>
  <ContactForm :newContact="newContact" @add-contact="addContact"/>
  <div class="flex">
    <div class="card one">
      <div>
      <!--SEARCH FORM-->
        <h2>Contact List</h2>
        <form action="submit" @submit.prevent="filter">
          <input type="text" @input="filter" v-model="search" placeholder="Search">
          <button type="submit">Search</button>
        </form>
      </div>
      <ContactList :contacts="filteredList" @display-info="openInfo"/>
    </div>
    <ContactInfo v-if="selectedContact" :contact="selectedContact" @deleting-contact="deleteContact" @open-edit="openEdit" />
    <EditForm v-if="editedContact" :contact="editedContact" @deleting-contact="deleteContact" @editing-contact="editContact" />
  </div>
</template>
