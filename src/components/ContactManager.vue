<template>
    <div>
      <div class="mb-3 d-flex justify-content-between align-items-center">
        <h2>Lista de contactos</h2>
        <div>
          <input type="text" v-model="searchQuery" class="form-control d-inline-block me-2" placeholder="Buscar por name y/o email" style="width: 300px;" />
          <button class="btn btn-primary" @click="addContact">Agendar Contacto</button>
        </div>
      </div>
  
      <table class="table table-bordered">
        <thead class="table btn-primary">
          <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Email</th>
            <th>Address</th>
            <th>Phone</th>
            <th>Country</th>
            <th>City</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="contact in filteredContacts" :key="contact.id">
            <td>{{ contact.id }}</td>
            <td>{{ contact.name }}</td>
            <td>{{ contact.email }}</td>
            <td>{{ contact.address }}</td>
            <td>{{ contact.phone }}</td>
            <td>{{ contact.country }}</td>
            <td>{{ contact.city }}</td>
            <td>
              <button class="btn btn-warning btn-sm me-1" @click="editContact(contact)">Editar</button>
              <button class="btn btn-danger btn-sm" @click="deleteContact(contact.id)">Eliminar</button>
            </td>
          </tr>
        </tbody>
      </table>
    
      <div v-if="editingContact" class="card mt-4">
        <div class="card-header">Edit Contact</div>
        <div class="card-body">
          <div class="mb-3">
            <input v-model="editingContact.name" class="form-control" placeholder="Name" />
          </div>
          <div class="mb-3">
            <input v-model="editingContact.email" class="form-control" placeholder="Email" />
          </div>
          <div class="mb-3">
            <input v-model="editingContact.address" class="form-control" placeholder="Address" />
          </div>
          <div class="mb-3">
            <input v-model="editingContact.phone" class="form-control" placeholder="Phone" />
          </div>
          <div class="mb-3">
            <input v-model="editingContact.country" class="form-control" placeholder="Country" />
          </div>
          <div class="mb-3">
            <input v-model="editingContact.city" class="form-control" placeholder="City" />
          </div>
          <button class="btn btn-success me-2" @click="saveContact">Guardar</button>
          <button class="btn btn-secondary" @click="cancelEdit">Cancelar</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        contacts:  JSON.parse(localStorage.getItem('contacts')) || [
                    {
                    id: 1,
                    name: "Alice Johnson",
                    email: "alice.johnson@example.com",
                    address: "123 Maple Street",
                    phone: "123-456-7890",
                    country: "USA",
                    city: "New York"
                    },
                    {
                    id: 2,
                    name: "Bob Smith",
                    email: "bob.smith@example.com",
                    address: "456 Oak Avenue",
                    phone: "987-654-3210",
                    country: "Canada",
                    city: "Toronto"
                    },
                    {
                    id: 3,
                    name: "Carol White",
                    email: "carol.white@example.com",
                    address: "789 Pine Road",
                    phone: "555-123-4567",
                    country: "UK",
                    city: "London"
                    },
                    {
                    id: 4,
                    name: "David Brown",
                    email: "david.brown@example.com",
                    address: "321 Elm Street",
                    phone: "444-555-6666",
                    country: "Australia",
                    city: "Sydney"
                    },
                    {
                    id: 5,
                    name: "Emily Davis",
                    email: "emily.davis@example.com",
                    address: "654 Spruce Lane",
                    phone: "333-444-5555",
                    country: "USA",
                    city: "Los Angeles"
                    }
                    ],
        searchQuery: '',
        editingContact: null, 
      };
    },
    computed: {
      filteredContacts() {
        return this.contacts.filter(contact =>
          contact.name.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
          contact.email.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      },     
    },
    methods: {
      addContact() {
        const newContact = {
          id: Date.now(),
          name: '',
          email: '',
          address: '',
          phone: '',
          country: '',
          city: '',
        };
        const seq=this.contacts.length;
        newContact.id=seq+1;
        this.contacts.push(newContact);
        this.updateLocalStorage();
        this.editingContact = newContact;
      },
      deleteContact(id) {
        this.contacts = this.contacts.filter(contact => contact.id !== id);
        this.updateLocalStorage();
      },
      editContact(contact) {
        this.editingContact = { ...contact };
      },
      saveContact() {
        const index = this.contacts.findIndex(contact => contact.id === this.editingContact.id);
        if (index !== -1) {
          this.contacts.splice(index, 1, this.editingContact);
        }
        this.updateLocalStorage();
        this.editingContact = null;
      },
      cancelEdit() {
        this.editingContact = null;
      },
      updateLocalStorage() {
      localStorage.setItem('contacts', JSON.stringify(this.contacts));
     },
    },
  };
  </script>
  
  <style>
  .table {
    margin-top: 20px;
  }
  .pagination {
  margin-top: 20px;
}
  </style>