<template>
    <div id='app'>
        <div class="main">
            <div class="left-bar" @click="changemode">
                <div v-for="conversation in conversationList" :key="conversation" >
                    <Conversation :conversation="conversation"/>
                </div>
            </div>
            <div v-if="conversationmode" class="chat-style">
                <div class="chat-container">
                    <div class="chat-box" v-for="each in messageList" :key="each">
                        <p v-if="user === each.user">{{each.message}}</p>
                        <p v-else id="theirs">{{each.message}}</p> 
                    </div>
                </div>
                <form @submit="handleSubmit">
                    <textarea v-model="message" type="text" id="input"></textarea>
                    <input v-if="this.submitIsVisible" id="submit" type="submit" value="Send">
                </form>
            </div>
            <!-- <div class="right-bar">
                <h2>Tools</h2>
                <p>New Features Coming soon!</p>
            </div> -->
        </div>
    </div>
</template>

<script>
import Conversation from '../Components/Conversation'
    export default {
        name: 'Chat',
        props: [
            'user',
        ],
        components: {
            Conversation,
        },
        data() {
            return {
                message: "",
                messageList: [],
                conversationList: [],
                count: 0,
                conversationmode: false,
                submitIsVisible: true,
                defaultmessage: `User ${this.user} is on a 5 minute timeout, please take some time to let things descalate`
            }
        },
        sockets: {
            connect(){

                },
            servermessage(data){
                this.addToMessageList(data)
            },
            timeout(data){
                if (this.user !== data.user){
                    this.toggleSubmit()
                    setTimeout(function () {
                        this.toggleSubmit()}.bind(this), 300000)
                        return alert(`${data.message}`)
                    }
                }
        },
        methods: {
            handleSubmit(event) {
                event.preventDefault()
                let message = this.message
                let badwords = /[****]/gi;
                let found = message.match(badwords)
                if (this.count >= 2){
                    this.toggleSubmit()
                    this.$socket.client.emit('timeout',{ message: this.defaultmessage, user: this.user})
                    setTimeout(function () {
                        this.toggleSubmit()}.bind(this), 300000)
                    return alert("The conversation seems to be escalating, in an unhealthy manner. There will be short 5 minute time out so cooler heads can prevail.")
                }
                if (found? found.length > 0: false){
                    this.count = this.count + 1
                    return alert("Your message includes some harsh language. Take time to reframe your statement, in a kind and generous manner.")
                }
                this.$socket.client.emit('message', {message, user: `${this.user}`});
                this.message = ""
            },
            addToMessageList(servermessage){
                this.messageList = [...this.messageList, servermessage]
            },
            toggleSubmit() {
                this.submitIsVisible = !this.submitIsVisible
            },
            changemode(){
                this.conversationmode = !this.conversationmode
            }
        },
        created(){
            fetch('http://localhost:4000/conversations')
            .then(response => response.json())
            .then(data => {
                this.conversationList = data
            })
        }
    }
</script>
<style lang="scss">
.main {
    padding-top: 1rem;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}
.left-bar {
    width: 100%;
    max-width: 300px;
    height: 500px;
    overflow: scroll;
    padding-right: 1rem;
    padding-bottom: 0;
    padding-top: 0;
    background-color: hsl(0, 100%, 100%);
    border-radius: 3px;
}
.right-bar {
    width: 100%;
    max-width: 300px;
    height: 500px;
    overflow: scroll;
}
.chat-style{
    margin-left: 1rem;
    margin-right: 1rem;
    left: 50%;
    width: 100%;
    max-width: 550px;
}
.chat-container {
    display: relative;
    background-color: azure;
    display: flex;
    flex-direction: column;
    width: 100%;
    min-width: 500px;
    height: 500px;
    overflow: scroll;
}
.chat-box {
    display: flex;
    width: 500px;
    margin: 2px;
    p {
        background-color: #beeafd62;
        border-radius: 20px;
        margin: 0px;
        padding: 1rem;
    }
}

#theirs {
    background-color: #ebebeb81;
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
    background-color: rgba(196, 231, 255, 0.459);
    border-radius: 1rem;
    padding: 0 .5rem;
}

#submit {
    background-color: rgb(70, 70, 70);
    color: white;
    border-radius: 12px;
}

</style>