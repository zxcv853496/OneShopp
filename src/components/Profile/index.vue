<template src = './template.html'>
  
</template>

<script>
export default {
    props: ['profile'],
    name: 'profile',
    computed: {
        msg() {
            if (this.profile.name) {
                return `Welcome ${this.profile.name}`
            }
            else {
                return `Login Facebook`
            }
        },
        profilePicture() {
            return (this.profile.id) ? `https://graph.facebook.com/${this.profile.id}/picture?width=300` : `/static/man.gif`
        },
        profileEmail() {
            if (this.profile.email){
                return `email : ${this.profile.email}`
            }
            else {
                return ``
            }
        },

    }
}
</script>

<style>

</style>