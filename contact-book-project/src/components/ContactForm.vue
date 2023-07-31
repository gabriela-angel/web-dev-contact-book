<script>
export default{
    name: 'ContactList',
    props: ['newContact'],
    methods: {
        addingContact: function (newContact) {
            this.$emit('add-contact', newContact)
        }
    }
}
</script>

<template>
    <div class="card">
        <h2>New Contact</h2>
        <form @submit.prevent="addingContact" id="add-form">
            <!--v-model = two-way data binding; you change the value by typing on the input OR by changing the data value-->
            <label for="first-name">First Name:</label>
            <input v-model="newContact.firstName" type="text" placeholder="First Name" name="first-name" required>
            <label for="last-name">Last Name:</label>
            <input v-model="newContact.lastName" type="text" placeholder="Last Name" name="last-name" required>
            <label for="email">Email:</label>
            <input v-model="newContact.email" type="text" placeholder="Email" name="email" required>
            <button type="submit">Add Contact</button>
        </form>
    </div>
</template>