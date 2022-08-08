<template>
  <v-container>
    <h1>Register</h1>
    <hr>
    <v-form v-model="valid">
    <v-container>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="user.username"
                    placeholder="Username"
                    :rules="rule"
                    required
                    label="Username"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="user.name"
                    placeholder="Name"
                    :rules="rule"
                    required
                    label="Name"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="user.phone"
                    placeholder="Phone"
                    :rules="rule"
                    required
                    label="Phone"
                    outlined
                >
                </v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-text-field
                    v-model="user.password"
                    placeholder="Password"
                    :rules="rule"
                    required
                    label="Password"
                    outlined
                    :append-icon="show1 ? 'mdi-eye-off' : 'mdi-eye'"
                    :type="show1 ? 'text' : 'password'"
                    @click:append="show1 = !show1"
                >
                </v-text-field>
            </v-col>
        </v-row>
    </v-container>
    </v-form>
    <v-btn
        outlined
        to="/"
    >
    Cancel
    </v-btn>
    <v-btn
        outlined
        @click="persist"
    >
    Save
    </v-btn>
  </v-container>
</template>

<script>
export default {
    name: 'RegisterPage',
    layout: 'login',
    data () {
        return {
            show1: false,
            valid: false,
            user: {
              id: null,
              name: null,
              username: null,
              phone: null,
              password: null
          },
          rule: [
              v => !!v || 'Required field'
          ]
        }
    },
  
    methods: {
        async persist () {
            try {
                //primeiro valida-se se o formulário está preenchido
                if (!this.valid) {
                    return this.$toast.warning('The registration form is not valid!')
                }
            
                let user = {
                    name: this.user.name,
                    username: this.user.username,
                    phone: this.user.phone,
                    password: this.user.password
                }
                let response = await this.$axios.$post('http://localhost:3333/users/register', user);
                if (response.type == "error") {
                    return this.$toast.info('There is already an user with this username!')
                }
                this.$toast.success('Registration successfully completed!');
                return this.$router.push("/");
            } catch (error) {
                this.$toast.error('An error occurred while registering!');
            }
        },
    }
}
</script>