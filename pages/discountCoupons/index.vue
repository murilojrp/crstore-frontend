<template>
  <v-container>
    <h1>Valid Discount Coupons</h1>
    <hr>
    <v-container>
        <v-row>
            <v-col>
            <v-btn
                outlined
                color="blue"
                @click="getDiscountCoupons"
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
                to="/discountCoupons/newDiscountCoupon"
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
         :items="discountCoupons"
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
    name: 'DiscountCouponsQueryPage',
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
                    text: 'Coupon',
                    align: 'center',
                    sortable: false,
                    value: 'coupon',
                },
                {
                    text: 'Discount',
                    align: 'center',
                    sortable: true,
                    value: 'discount',
                },
                { text: "", value: "actions" }
            ],
            discountCoupons: []
        }
    },
    created () {
        this.getDiscountCoupons()
    },
    methods: {
        async getDiscountCoupons () {
            this.discountCoupons = await this.$axios.$get('http://localhost:3333/discountCoupons');
        },
        async deleteItem (discountCoupon) {
            try {
            if (confirm(`Do you want to delete this Coupon?`)) {
                let response = await this.$axios.$post('http://localhost:3333/discountCoupons/destroy', { id: discountCoupon.id });
                this.$toast.success(`Coupon:${discountCoupon.coupon} successfully deleted!`);
                this.getDiscountCoupons ();
            } 
            } catch (error) {
            this.$toast.error('An error occurred while fulfilling the request. Contact the ADM.')
        }
     },
    async editItem (discountCoupon) {
      this.$router.push({
        name: 'discountCoupons-newDiscountCoupon',
        params: { id: discountCoupon.id }
      });
    }
  }
}
</script>