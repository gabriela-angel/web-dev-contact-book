<script>
export default{
    name: 'ContactList',
    props: ['contact'],
    methods: {
        deletingContact: function () {
            this.$emit('deleting-contact', this.contact)
        },

        openEditForm: function () {
            this.$emit('open-edit', this.contact)
        }
    }
}
</script>

<template>
    <div class="card two">
        <div>
            <h2>Contact Info</h2>
            <div class="info-flex">
                <h3>First Name: </h3>
                <p class="cap">{{ contact.firstName }} </p>
                <h3>Last Name:</h3>
                <p class="cap">{{ contact.lastName }}</p>
                <h3>Email:</h3>
                <p>{{ contact.email }}</p>
                <div>
                    <button type="button" @click="deletingContact" >Delete</button>
                    <button class="edit-button" type="button" @click="openEditForm">Edit</button>
                </div>
                
            </div>
            
        </div>
    </div>
</template>