<template>
  <v-container fluid>
    <v-row>
      <v-col cols="12" md="3">
        <v-card class="pa-4">
          <v-card-title class="headline">
            Filtros
          </v-card-title>
          <v-card-text>
            <v-text-field v-model="filters.name" label="Nombre" outlined dense />
            <v-text-field v-model="filters.category" label="Categoría" outlined dense />
            <v-text-field v-model="filters.brand" label="Marca" outlined dense />
            <v-text-field v-model="filters.priceRange" label="Rango de Precio" outlined dense />
            <v-btn color="primary" @click="applyFilters">
              Aplicar Filtros
            </v-btn>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="12" md="9">
        <v-row>
          <v-col cols="12">
            <v-btn color="primary" @click="goToSaleForm">
              Nueva Factura
            </v-btn>
          </v-col>
          <v-col cols="12">
            <v-card class="pa-4 mb-4">
              <v-card-title class="headline">
                Producto Destacado
              </v-card-title>
              <v-card-text>
                <v-row>
                  <v-col cols="12" md="4">
                    <v-img :src="featuredProduct.image" aspect-ratio="1.7" />
                  </v-col>
                  <v-col cols="12" md="8">
                    <h3>{{ featuredProduct.name }}</h3>
                    <p>{{ featuredProduct.description }}</p>
                    <v-col cols="6">
                      <p class="text-h6 font-weight-bold">
                        <strong>Precio</strong>: {{ `${featuredProduct.price}USD` }}
                      </p>
                      <p class="text-body-2 text--line-through">
                        <v-icon>
                          mdi-currency-usd
                        </v-icon>
                        Dolares Estadounidenses
                      </p>
                    </v-col>
                    <v-rating value="4.5" readonly />
                    <v-chip>{{ featuredProduct.category }}</v-chip>
                    <v-chip>{{ featuredProduct.brand }}</v-chip>
                    <v-chip>{{ featuredProduct.weight }} kg</v-chip>
                    <v-chip>{{ featuredProduct.dimensions }} cm</v-chip>
                    <v-chip>{{ featuredProduct.color }} Hex</v-chip>
                    <v-chip>{{ featuredProduct.material }}</v-chip>
                    <v-btn color="primary" @click="buyProduct(featuredProduct.id)">
                      Comprar
                    </v-btn>
                  </v-col>
                </v-row>
              </v-card-text>
            </v-card>
          </v-col>
          <v-col cols="12">
            <v-text-field
              v-model="searchQuery"
              label="Buscar Producto"
              outlined
              dense
              append-icon="mdi-magnify"
              @keyup.enter="searchProduct"
            />
          </v-col>
          <v-col cols="12">
            <v-row>
              <v-col v-for="product in filteredProducts" :key="product.id" cols="12" md="4">
                <v-card class="mb-4">
                  <v-img :src="product.image" aspect-ratio="1.7" />
                  <v-card-title>{{ product.name }}</v-card-title>
                  <v-card-text>
                    <p>{{ product.description }}</p>
                    <p><strong>Precio:</strong> {{ product.price }}</p>
                    <v-rating v-model="product.rating" readonly />
                    <v-chip>{{ product.category }}</v-chip>
                    <v-chip>{{ product.brand }}</v-chip>
                    <v-chip>{{ product.weight }} kg</v-chip>
                    <v-chip>{{ product.dimensions }}</v-chip>
                    <v-chip>{{ product.color }}</v-chip>
                    <v-chip>{{ product.material }}</v-chip>
                    <v-btn color="primary" @click="buyProduct(product.id)">
                      Comprar
                    </v-btn>
                  </v-card-text>
                </v-card>
              </v-col>
            </v-row>
          </v-col>
          <v-col cols="12">
            <v-data-table
              :headers="headers"
              :items="filteredProducts"
              :items-per-page="5"
              class="elevation-1"
            />
          </v-col>
          <v-col cols="12">
            <v-card class="pa-4 mb-4">
              <v-card-title class="headline">
                Reseñas
              </v-card-title>
              <v-card-text>
                <!-- Reviews will be implemented later -->
                <p>Aquí se mostrarán las reseñas de los productos.</p>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  layout: 'dashboard',
  data () {
    return {
      products: [],
      allProducts: [],
      filteredProducts: [],
      featuredProduct: {},
      searchQuery: '',
      filters: {
        name: '',
        category: '',
        brand: '',
        priceRange: ''
      },
      headers: [
        { text: 'Nombre', align: 'center', sortable: true, value: 'name' },
        { text: 'Precio', align: 'center', sortable: true, value: 'price' },
        { text: 'Descripción', align: 'center', sortable: true, value: 'description' },
        { text: 'Stock', align: 'center', sortable: true, value: 'stock' },
        { text: 'Categoría', align: 'center', sortable: true, value: 'category' },
        { text: 'Marca', align: 'center', sortable: true, value: 'brand' },
        { text: 'Peso', align: 'center', sortable: true, value: 'weight' },
        { text: 'Dimensiones', align: 'center', sortable: true, value: 'dimensions' },
        { text: 'Color', align: 'center', sortable: true, value: 'color' },
        { text: 'Material', align: 'center', sortable: true, value: 'material' }
      ]
    }
  },
  async mounted () {
    await this.fetchProducts()
  },
  methods: {
    async fetchProducts () {
      const res = await this.$axios.get('/product/all')
      if (res && res.data && res.data.success) {
        this.allProducts = res.data.products
        this.products = res.data.products.slice(0, 6) // Display first 6 products as cards
        this.featuredProduct = res.data.products[0] // Set the first product as the featured product
        this.filteredProducts = res.data.products // Initialize filteredProducts with all products
      }
    },
    applyFilters () {
      this.filteredProducts = this.allProducts.filter((product) => {
        const matchesName = this.filters.name ? product.name.toLowerCase().includes(this.filters.name.toLowerCase()) : true
        const matchesCategory = this.filters.category ? product.category.toLowerCase().includes(this.filters.category.toLowerCase()) : true
        const matchesBrand = this.filters.brand ? product.brand.toLowerCase().includes(this.filters.brand.toLowerCase()) : true
        const matchesPriceRange = this.filters.priceRange ? this.isWithinPriceRange(product.price, this.filters.priceRange) : true
        return matchesName && matchesCategory && matchesBrand && matchesPriceRange
      })
    },
    isWithinPriceRange (price, priceRange) {
      const [min, max] = priceRange.split('-').map(Number)
      return price >= min && price <= max
    },
    searchProduct () {
      this.filteredProducts = this.allProducts.filter(product => product.name.toLowerCase().includes(this.searchQuery.toLowerCase()))
    },
    goToSaleForm () {
      this.$router.push('/dashboard/ventas/saleForm')
    },
    buyProduct (productId) {
      this.$router.push({ path: '/dashboard/ventas/saleForm', query: { productId } })
    }
  }
}
</script>

<style scoped>
.headline {
  font-weight: bold;
}
</style>
