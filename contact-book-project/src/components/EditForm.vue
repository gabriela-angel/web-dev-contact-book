<script>
export default{
    name: 'EditForm',
    props: ['contact'],
    methods: {
        deletingContact: function () {
            this.$emit('deleting-contact', this.contact)
        },

        editingContact: function () {
            this.$emit('editing-contact')
        }
    }
}
</script>

<template>
    <div class="card two">
        <div>
            <h2>Edit Info</h2>
            <form @submit.prevent="editingContact" id="edit-form">
                <label for="first-name">First Name:</label>
                <input v-model="contact.firstName" type="text" placeholder="First Name" name="first-name" required>
                <label for="last-name">Last Name:</label>
                <input v-model="contact.lastName" type="text" placeholder="Last Name" name="last-name" required>
                <label for="email">Email:</label>
                <input v-model="contact.email" type="text" placeholder="Email" name="email" required>
            </form>
            <div>
                <button type="button" @click="deletingContact" >Delete</button>
                <button type="submit" form="edit-form">Save</button>
            </div>
        </div>
    </div>
</template>