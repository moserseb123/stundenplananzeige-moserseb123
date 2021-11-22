<template>
    <div>
        <div id="header">
            <img src="../assets/logo.png" alt="">
            <h1>Stundentafel für {{username}}</h1>
        </div>

        <div id="error" v-if="errormessage.value!=''">
            <h2>Fehler</h2>
            {{errormessage}}
        </div>

    </div>
</template>

<script>
    export default {
        name: "Header",
        props: {
            username: {
                String,
                required: true
            },
            errormessage: {
                String,
                required: false,
                deafault:''
            }
        },
    }
</script>

<style scoped>
    #header {
        align-items: baseline;
        grid-gap: 21px;
        justify-content: center;
        display: flex;
        align-items: center;
    }

    #header h1 {
        margin-left: 1em;
    }

    #error {
        color:red;
        background-color: greenyellow;
        margin-top: 20px;
        text-align: center;
        margin-left: 40%;
        margin-right: 40%;
    }
</style>