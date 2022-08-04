<template>
  <v-container>
    <h1>Categories Query</h1>
    <hr>
    <v-container>
        <v-row>
            <v-col>
            <v-btn
                outlined
                color="blue"
                @click="getCategories"
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
                to="/categories/newCategory"
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
         :items="categories"
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
    name: 'CategoriesQueryPage',
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
                { text: "", value: "actions" }
            ],
            categories: []
        }
    },
    created () {
        this.getCategories()
    },
    methods: {
        async getCategories () {
            this.categories = await this.$axios.$get('http://localhost:3333/categories');
        },
        async deleteItem (category) {
            try {
            if (confirm(`Do you want to delete category:${category.name}?`)) {
                let response = await this.$axios.$post('http://localhost:3333/categories/destroy', { id: category.id });
                this.$toast.success(`Category:${category.name} successfully deleted!`);
                this.getCategories ();
            } 
            } catch (error) {
            this.$toast.error('An error occurred while fulfilling the request. Contact the ADM.')
        }
     },
    async editItem (category) {
      this.$router.push({
        name: 'categories-newCategory',
        params: { id: category.id }
      });
    }
  }
}
</script>