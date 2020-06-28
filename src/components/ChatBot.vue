<template>
    <div class="row chatarea">
        <div class="col-md-12 header">
            <img class="mx-auto d-block" src="https://finchatbot.com/wp-content/uploads/2017/06/logo.png" alt="finchatbot" width="30%">
        </div>
        <div v-if="messages.length > 0" class="col-md-12">
            <div v-for="(message, index) in messages" :key="index">
                <div v-if="message.sender === 'bot'">
                    <div class="row justify-content-start mt-3">
                        <div class="col-1">
                            <img src="https://s3-eu-west-1.amazonaws.com/finchatbot.com/logo-simple-mobile-version.png" alt="chatbot" width="100%">    
                        </div>  
                        <div class="col-auto message bot-message">
                            {{ message.message }}
                        </div>
                    </div>
                </div>
                <div v-else>
                    <div class="row justify-content-end mt-3">
                        <div class="col-auto message user-message">
                            {{ message.message }}
                        </div>
                        <div class="col-1">
                            <img src="https://i7.pngguru.com/preview/555/557/138/computer-icons-user-profile-woman-avatar-rent-thumbnail.jpg" alt="user" width="100%">
                        </div>
                    </div>
                </div>
            </div>
            <div class="mt-4">
                <div v-if="isLoading">
                    <img src="https://cdn.dribbble.com/users/225707/screenshots/2958729/attachments/648705/straight-loader.gif" width="10%">
                </div>
                <div class="input-group mb-3">
                    <input type="text" class="form-control" aria-label="Recipient's username" aria-describedby="basic-addon2" v-model="message" @keyup.enter="sendMessage">
                    <div class="input-group-append">
                        <button :disabled="message ? false : true" @click="sendMessage" class="btn btn-outline-secondary" type="button">Send</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'ChatBot',
    data() {
        return {
            message: '',
            user: 'bank-chieh-732',
            status: 'incomplete',
            isLoading: false,
            messages: []
        }
    },
    created() {
        axios.get('http://localhost:3300').then(response => {
            this.messages.push(formMessage(response.data.message, 'bot'));
        }).catch(err => console.log(err));
    },
    methods: {
        sendMessage() {
            this.isLoading = true;
            this.message.toLowerCase().indexOf('bye') > -1 ? this.status = 'completed' : this.status = 'incomplete';
            this.messages.push(formMessage(this.message, 'client'));
            axios.post('http://localhost:3300', {message: this.message, user: this.user}).then(response => {
                this.messages.push(formMessage(response.data.message, 'bot'));
                this.isLoading = false;
                axios.post('http://localhost:3300/saveMessage', {history: this.messages, user: this.user, status: this.status}).then(response => console.log(response.data)).catch(err => console.log(err));
            }).catch(err => console.log(err));
            this.message = '';
        }
    }
}
const formMessage = (message, sender) => {
    return {
        message,
        sender,
        time: new Date().toTimeString()
    }
}
</script>

<style scoped>
    .header {
        padding: 30px;
    }
    .chatarea {
        border: 1px solid #ddd;
        border-radius: 10px;
    }
    .message {
        display: inline-block;
        color: #fff;
        padding: 12px;
        border-bottom-right-radius: 10px;
        border-bottom-left-radius: 10px;
    }
    .bot-message {
        background: #293160;
        border-top-right-radius: 10px;
    }
    .user-message {
        background: #40a7a9;
        border-top-left-radius: 10px;
    }
</style>
