<template>
    <div id='app'>
        <div class="chat-style">
            <div class="chat-container">
                <div class="chat-box" v-for="each in messageList" :key="each">
                    <p v-if="socketid === each.socketid">{{each.message}}</p>
                    <p v-else class="right">{{each.message}}</p> 
                </div>
            </div>
            <form @submit="handleSubmit">
                <textarea v-model="message" type="text" id="input"></textarea>
                <input id="submit" type="submit" value="Send">
            </form>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Chat',
        data() {
            return {
                message: "",
                messageList: [],
                socketid: ""
            }
        },
        sockets: {
            connect(){
                console.log('connected',this)
                this.socketid = this.$socket.client.id
                },
            servermessage(data){
                console.log(data)
                this.addToMessageList(data)
            }
        },
        methods: {
            handleSubmit(event) {
                event.preventDefault()
                let message = this.message
                this.$socket.client.emit('message', {message, socketid: this.socketid});
                this.message = ""
            },
            addToMessageList(servermessage){
                this.messageList = [...this.messageList, servermessage]
            }
        }
    }
</script>
<style lang="scss">

.chat-style{
    width: 100%;
    max-width: 500px;
}
.chat-container {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 500px;
    overflow: scroll;
}
.right {
    text-align: right;
}
.chat-box {
    display: flex;
    width: 500px;

    p {
        background-color: rgba(135, 197, 197, 0.8);
        border-radius: 20px;
        margin: 0px;
        padding: 1rem;
    }
}

form {
    width: 100%;
    display: flex;
}

#input {
    align-self: center;
    display: inline-block;
    overflow-wrap: break-word;
    width: 100%;
}

</style>