
<template>
    <h1>Login Formular</h1>
    <div id="container">
        <Form id="container-form" @submit.prevent="processLogin">
            <br>
            <field
                    id="email"
                    type="email"
                    name="email"
                    placeholder="Email"
                    v-model="email"
                    :rules="validateEmail"
            />
            <br>
            <br>
            <field
                    id="password"
                    type="password"
                    name="password"
                    placeholder="Passwort"
                    v-model="password"
            />
            <br>
            <a href="" id="forgotPasswd">Passwort vergessen?</a>
            <br>
            <br>
            <input
                    id="loginBtn"
                    type="submit"
                    value="Login"
            />
            <br>
        </Form>


    </div>
</template>

<script>
    import {ref} from 'vue';
    import {Field,Form} from 'vee-validate';

    export default {
        components: {
            Field,
            Form
        },
        setup(props,context) {
            let email = ref('');
            let password = ref('');

            function processLogin(values) {
                console.log("Login::processLogin, Values: "+values);
                context.emit("loginDone");
            }

            function validateEmail(value) {
                // if the field is empty
                if (!value) {
                    return 'This field is required';
                }
                if(value.length < 4) return !length

                // if the field is not a valid email
                const regex = /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i;
                if (!regex.test(value)) {
                    return 'This field must be a valid email';
                }
                // All is good
                return true;
            }

            return {
                email,
                password,
                processLogin,
                validateEmail,
            }
        }
    }
</script>

<style scoped>
    #container{
        display: flex;
        flex-direction: column;
    }
    #loginBtn{
        background-color: blue;
        color: white;
        transform: scale(2);
        font-size: small;
        border: none;
        font-size: 5px;
        width: 50px;
        height: 15px;
    }
    #email{
        width: 200px;
        height: 25px;
    }

    #password{
        width: 200px;
        height: 25px;
    }

    #forgotPasswd{
        font-size: small;

    }

</style>