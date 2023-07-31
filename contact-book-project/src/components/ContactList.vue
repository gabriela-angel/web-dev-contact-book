<script>
export default{
    name: 'ContactList',
    props: ['contacts'],
    methods: {
        displayInfo: function(contact) {
            this.$emit('display-info', contact)
        }
    }
}
</script>

<template>
    <div class="list-container">
        <div v-for="contact in contacts" :key="contact.id" @click="displayInfo(contact)" class="contact">
            <p class="cap">{{ contact.lastName }}, {{ contact.firstName }}</p>
        </div>
    </div>
</template>