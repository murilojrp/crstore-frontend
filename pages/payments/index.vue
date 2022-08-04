<template>
  <v-container>
    <h1>Accepted Payment Methods Query</h1>
    <hr>
    <v-container>
        <v-row>
            <v-col>
            <v-btn
                outlined
                color="blue"
                @click="getPayments"
            >
                Search
                <v-icon
                    style="margin-left:5%"
                >
                    mdi-magnify
                </v-icon>
            </v-btn>
            <v-btn
                color="green"
                fab
                style="margin-left:1%"
                to="/payments/newPayment"
            >
                <v-icon>
                    mdi-plus
                </v-icon>
            </v-btn>
            </v-col>
        </v-row>
    </v-container>
    <v-container>
        <v-data-table
         :headers="headers"
         :items="payments"
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
  </v-container>
</template>

<script>
export default {
    name: 'PaymentsQueryPage',
    data () {
        return {
            headers: [
                {
                    text: 'Code',
                    align: 'center',
                    sortable: true,
                    value: 'id',
                },
                {
                    text: 'Method',
                    align: 'center',
                    sortable: true,
                    value: 'method',
                },
                { text: "", value: "actions" }
            ],
            payments: []
        }
    },
    created () {
        this.getPayments()
    },
    methods: {
        async getPayments () {
            this.payments = await this.$axios.$get('http://localhost:3333/payments');
        },
        async deleteItem (payment) {
            try {
            if (confirm(`Do you want to delete payment:${payment.method}?`)) {
                let response = await this.$axios.$post('http://localhost:3333/payments/destroy', { id: payment.id });
                this.$toast.success(`Payment:${payment.method} successfully deleted!`);
                this.getPayments ();
            } 
            } catch (error) {
            this.$toast.error('An error occurred while fulfilling the request. Contact the ADM.')
        }
     },
    async editItem (payment) {
      this.$router.push({
        name: 'payments-newPayment',
        params: { id: payment.id }
      });
    }
  }
}
</script>