<template>
  <v-container>
    <h1>Items Query</h1>
    <hr>
    <v-container>
        <v-row>
            <v-col>
            <v-btn
                outlined
                color="blue"
                @click="getItems"
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
                to="/items/newItem"
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
         :items="items"
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
    name: 'ItemsQueryPage',
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
                    text: 'Name',
                    align: 'center',
                    sortable: true,
                    value: 'name',
                },
                {
                    text: 'Category',
                    align: 'center',
                    sortable: false,
                    value: 'category.name',
                },
                {
                    text: 'Price',
                    align: 'center',
                    sortable: false,
                    value: 'price',
                },
                { text: "", value: "actions" }
            ],
            items: []
        }
    },
    created () {
        this.getItems();
    },
    methods: {
        async getItems () {
            this.items = await this.$axios.$get('http://localhost:3333/items');
        },
        async deleteItem (item) {
            try {
            if (confirm(`Do you want to delete item:${item.name}?`)) {
                let response = await this.$axios.$post('http://localhost:3333/items/destroy', { id: item.id });
                this.$toast.success(`Item:${item.name} successfully deleted!`)
                this.getItems();
            } 
            } catch (error) {
            this.$toast.error('An error occurred while fulfilling the request. Contact the ADM.')
        }
     },
    async editItem (item) {
      this.$router.push({
        name: 'items-newItem',
        params: { id: item.id }
      });
    }
  }
}
</script>