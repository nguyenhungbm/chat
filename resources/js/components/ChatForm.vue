<template>
    <div class="input-group">
        <input id="btn-input" type="text" name="message" class="form-control input-sm" placeholder="Type your message here..." v-model="newMessage" @keyup.enter="sendMessage">
        
        <span class="input-group-btn">
            <button class="btn btn-primary btn-sm" id="btn-chat" @click="sendMessage">
                Send
            </button>
        </span>
    </div>
</template>

<script>
    export default {
        props: ['user'],

        data() {
            return {
                newMessage: ''
            }
        },
        created() {
        this.fetchMessages();

        Echo.private('Messages')
            .listen('MessageSent', (e) => {
                alert(1);
                this.messages.push({
                    message: e.message.message,
                    user: e.user
                });
            });
    },

    methods: {
        fetchMessages() {
            axios.get('/messages').then(response => {
                this.messages = response.data;
            });
        },
        addMessage(message) {
            this.messages.push(message);

            axios.post('/messages', message).then(response => {});
        },
        sendMessage() {
            var data = {
                user : this.user,
                message : this.newMessage
            };
            axios.post('/messages', data).then(response => {
            });

            this.$emit('messagesent', {
                user: this.user,
                message: this.newMessage
            });

            this.newMessage = ''
        }
    }    
}
</script>
