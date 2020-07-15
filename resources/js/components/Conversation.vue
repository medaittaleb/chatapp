<template>
    <div class="conversation">
        <h1>{{ contact ? contact.name : "Select a Contact" }}</h1>
        <MessagesFeed :contact="contact" :messages="messages" />
        <MessageComposer @send="sendMessage" />
    </div>
</template>

<script>
    import MessageComposer from './MessageComposer';
    import MessagesFeed from './MessagesFeed';

    export default {
        props: {
            contact: {
                type: Object,
                default: null
            },
            messages: {
                type: Array,
                default: []
            }
        },
        methods: {
            sendMessage(text){
                if(!this.contact){
                    return;
                }
                axios.post('/conversation/send',{
                    contact_id: this.contact.id,
                    text: text
                }).then((resp) => {
                    this.$emit('newmsg', resp.data);
                })
            }
        },
        components: {
            MessagesFeed,
            MessageComposer
        }

    }
</script>

<style lang="scss" scoped>
.conversation {
    flex: 5;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    h1 {
        font-size: 20px;
        padding: 10px;
        margin: 0;
        border-bottom: 1px dashed lightgray;
    }
}
</style>
