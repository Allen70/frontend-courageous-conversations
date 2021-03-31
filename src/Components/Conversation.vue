<template>
    <div class="conversation">
          <p>{{conversation.name}}</p>
          <button>✔️</button>
    </div>
</template>

<script>
export default {
    name: "Conversation",
    props: ['conversation']
}
</script>

<style scoped>

.conversation {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
}

button {
    margin-left: .5rem;
    border: none;
    background: none;
    height: 20px;
    box-shadow: 0px 0px 1px 1px black;
    text-align: center;
}

</style>