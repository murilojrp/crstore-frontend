<template>
  <v-container>
    <h1>Your Addresses</h1>
    <hr>
    <br>
    <v-container>
        <v-data-table
         :headers="headers"
         :items="addresses"
         :items-per-page="10"
         class="elevation-1"
        >
        <template v-slot:item.actions="{ item }">
            <v-icon
                small
                class="mr-2"
                @click="editItem(item)"
                color="blue"
            >
                mdi-pencil
            </v-icon>
            <v-icon
                small
                @click="deleteItem(item)"
                color="red"
            >
                mdi-delete
            </v-icon>
        </template>
<template v-slot:no-data>
  <v-btn
    color="primary"
    @click="initialize"
  >
    Reset
  </v-btn>
</template>
        </v-data-table>
    </v-container>
    <v-container>
        <v-row>
            <v-col>
            <v-btn
                color="green"
                fab
                style="margin-left:50%"
                to="/users/newAddress"
            >
                <v-icon>
                    mdi-plus
                </v-icon>
            </v-btn>
            </v-col>
        </v-row>
    </v-container>
  </v-container>
</template>

<script>
export default {
    name: 'AddressesQueryPage',
    layout: 'user',
    data () {
        return {
            headers: [
                {
                    text: 'Street',
                    align: 'center',
                    sortable: true,
                    value: 'street',
                },
                {
                    text: 'Neighborhood',
                    align: 'center',
                    sortable: true,
                    value: 'neighborhood',
                },
                {
                    text: 'Number',
                    align: 'center',
                    sortable: true,
                    value: 'number',
                },
                {
                    text: 'Complement',
                    align: 'center',
                    sortable: true,
                    value: 'complement',
                },
                {
                    text: 'Address',
                    align: 'center',
                    sortable: true,
                    value: 'address',
                },
                { text: "", value: "actions" }
            ],
            addresses: []
        }
    },
    created () {
        this.getAddresses()
    },
    methods: {
        async getAddresses () {
            let response = await this.$api.get('/addresses');
            this.addresses = response.data;
        },
        async deleteItem (address) {
            try {
            if (confirm(`Do you want to delete this address?`)) {
                let response = await this.$api.post('/addresses/destroy', { id: address.id });
                this.$toast.success(`Address successfully deleted!`);
                this.getAddresses ();
            } 
            } catch (error) {
            this.$toast.error('An error occurred while fulfilling the request. Contact the ADM.')
        }
     },
    async editItem (address) {
      this.$router.push({
        name: 'users-newAddress',
        params: { id: address.id }
      });
    }
  }
}
</script>