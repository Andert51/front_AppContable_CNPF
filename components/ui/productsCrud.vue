<template>
  <div class="pa-3 ma-3">
    <v-row align="center" justify="center">
      <h1>Administracion de Productos</h1>
    </v-row>
    <v-row align="center" justify="end">
      <v-btn color="#3282B8" @click="dialogAdd = true">
        <v-icon color="white">
          mdi-plus
        </v-icon>
        <span style="text-transform: none;" class="add-btn-text">
          Agregar Nuevo Producto
        </span>
      </v-btn>
    </v-row>
    <v-row align="center" justify="center">
      <v-data-table
        :headers="headers"
        :items="products_data"
        item-key="name"
        class="styled-table"
      >
        <template #[`item.actions`]="{ item }">
          <v-tooltip bottom color="orange">
            <template #activator="{ on, attrs}">
              <v-btn
                icon
                color="warning"
                v-bind="attrs"
                @click="updateProductMid(item)"
                v-on="on"
              >
                <v-icon>
                  mdi-pencil-box-outline
                </v-icon>
              </v-btn>
            </template>
            <span>Actualizar Producto: {{ item.name }}</span>
          </v-tooltip>
          |
          <v-tooltip bottom color="red">
            <template #activator="{ on, attrs}">
              <v-btn
                icon
                color="red"
                v-bind="attrs"
                @click="deleteProductMid(item.id)"
                v-on="on"
              >
                <v-icon>
                  mdi-trash-can
                </v-icon>
              </v-btn>
            </template>
            <span>Eliminar Producto: {{ item.name }}</span>
          </v-tooltip>
        </template>
      </v-data-table>
    </v-row>
    <v-dialog v-model="dialogAdd" width="600">
      <v-card class="pa-5 rounded-card" width="600" color="white">
        <v-card-title class="text-h5 text-center mb-4" style="color: #1B262C;">
          Agregar Nuevo Producto
        </v-card-title>

        <v-card-text>
          <v-form ref="form">
            <v-row dense>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newProduct.name"
                  label="Nombre"
                  outlined
                  dense
                  class="custom-input"
                  prepend-icon="mdi-tag"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newProduct.price"
                  label="Precio"
                  outlined
                  dense
                  class="custom-input"
                  prepend-icon="mdi-currency-usd"
                  required
                />
              </v-col>
              <v-col cols="12">
                <v-text-field
                  v-model="newProduct.description"
                  label="Descripción"
                  outlined
                  dense
                  class="custom-input"
                  prepend-icon="mdi-text"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newProduct.stock"
                  label="Stock"
                  outlined
                  dense
                  class="custom-input"
                  prepend-icon="mdi-package-variant"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newProduct.category"
                  label="Categoría"
                  outlined
                  dense
                  class="custom-input"
                  prepend-icon="mdi-shape"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newProduct.brand"
                  label="Marca"
                  outlined
                  dense
                  class="custom-input"
                  prepend-icon="mdi-label"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newProduct.weight"
                  label="Peso"
                  outlined
                  dense
                  class="custom-input"
                  prepend-icon="mdi-weight"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newProduct.dimensions"
                  label="Dimensiones"
                  outlined
                  dense
                  class="custom-input"
                  prepend-icon="mdi-ruler"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newProduct.color"
                  label="Color"
                  outlined
                  type="color"
                  dense
                  class="custom-input"
                  prepend-icon="mdi-palette"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newProduct.material"
                  label="Material"
                  outlined
                  dense
                  class="custom-input"
                  prepend-icon="mdi-texture"
                  required
                />
              </v-col>
              <v-col cols="12">
                <v-file-input
                  v-model="newProduct.image"
                  label="Imagen del Producto"
                  outlined
                  dense
                  prepend-icon="mdi-camera"
                  class="custom-input"
                />
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>

        <v-card-actions class="justify-center mt-4">
          <v-btn color="grey darken-1" class="cancel-btn mx-2" @click="dialogAdd = false">
            Cancelar
          </v-btn>
          <v-btn color="blue darken-1" class="custom-btn mx-2" @click="addNewProduct">
            Agregar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="dialogUpdate" width="600">
      <v-card class="pa-6 rounded-update-card" width="650" color="grey lighten-4">
        <v-card-title class="text-h5 text-center mb-5" style="color: #34495E;">
          Actualizar Producto
        </v-card-title>

        <v-card-text>
          <v-form ref="form">
            <v-row dense>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="selectedProduct.name"
                  label="Nombre"
                  outlined
                  dense
                  class="update-input"
                  prepend-icon="mdi-tag"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="selectedProduct.price"
                  label="Precio"
                  outlined
                  dense
                  class="update-input"
                  prepend-icon="mdi-currency-usd"
                  required
                />
              </v-col>
              <v-col cols="12">
                <v-text-field
                  v-model="selectedProduct.description"
                  label="Descripción"
                  outlined
                  dense
                  class="update-input"
                  prepend-icon="mdi-text"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="selectedProduct.stock"
                  label="Stock"
                  outlined
                  dense
                  class="update-input"
                  prepend-icon="mdi-package-variant"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="selectedProduct.category"
                  label="Categoría"
                  outlined
                  dense
                  class="update-input"
                  prepend-icon="mdi-shape"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="selectedProduct.brand"
                  label="Marca"
                  outlined
                  dense
                  class="update-input"
                  prepend-icon="mdi-label"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="selectedProduct.weight"
                  label="Peso"
                  outlined
                  dense
                  class="update-input"
                  prepend-icon="mdi-weight"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="selectedProduct.dimensions"
                  label="Dimensiones"
                  outlined
                  dense
                  class="update-input"
                  prepend-icon="mdi-ruler"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="selectedProduct.color"
                  label="Color"
                  outlined
                  type="color"
                  dense
                  class="update-input"
                  prepend-icon="mdi-palette"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="selectedProduct.material"
                  label="Material"
                  outlined
                  dense
                  class="update-input"
                  prepend-icon="mdi-texture"
                  required
                />
              </v-col>
              <v-col cols="12">
                <v-file-input
                  v-model="selectedProduct.image"
                  label="Imagen del Producto"
                  outlined
                  dense
                  prepend-icon="mdi-camera"
                  class="update-input"
                />
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>

        <v-card-actions class="d-flex justify-center mt-4">
          <v-btn color="grey darken-1" class="cancel-btn mx-2" @click="dialogUpdate = false">
            Cancelar
          </v-btn>
          <v-btn color="teal darken-1" class="update-btn mx-2" @click="updateProduct">
            Actualizar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog
      v-model="dialogDelete"
      max-width="400"
      persistent
    >
      <v-card class="rounded-lg elevation-3">
        <!-- Título del diálogo -->
        <v-card-title class="text-h6 font-weight-bold text-center text-primary">
          <v-icon color="red" class="mr-2">
            mdi-delete
          </v-icon>
          Eliminar Producto
        </v-card-title>

        <!-- Mensaje del diálogo -->
        <v-card-text class="text-center text-body-1 text-gray-600">
          <v-icon color="blue" class="mb-2" size="36">
            mdi-alert-circle
          </v-icon>
          <p>¿Está seguro de que desea eliminar este Producto? Esta acción no se puede deshacer.</p>
        </v-card-text>

        <!-- Botones de acción -->
        <v-card-actions class="justify-center">
          <v-btn
            color="grey lighten-1"
            class="text-none"
            @click="dialogDelete = false"
          >
            Cancelar
          </v-btn>
          <v-btn
            color="red"
            dark
            class="text-none"
            @click="deleteProduct"
          >
            Eliminar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data () {
    return {
      products_data: [],
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
        { text: 'Material', align: 'center', sortable: true, value: 'material' },
        { text: 'Acciones', align: 'center', sortable: false, value: 'actions' }
      ],
      dialogAdd: false,
      dialogUpdate: false,
      dialogDelete: false,
      newProduct: {
        name: '',
        price: '',
        description: '',
        stock: '',
        category: '',
        image: null,
        brand: '',
        weight: '',
        dimensions: '',
        color: '',
        material: ''
      },
      selectedProduct: {
        name: '',
        price: '',
        description: '',
        stock: '',
        category: '',
        image: null,
        brand: '',
        weight: '',
        dimensions: '',
        color: '',
        material: ''
      },
      id: null
    }
  },
  mounted () {
    this.getProducts()
  },
  methods: {
    async getProducts () {
      const res = await this.$axios.get('/product/all')
      if (res && res.data && res.data.success) {
        this.products_data = res.data.products
      }
    },
    deleteProductMid (idDel) {
      this.id = idDel
      this.dialogDelete = true
    },
    async deleteProduct () {
      const res = await this.$axios.delete(`/product/delete/${this.id}`)
      if (res && res.data && res.data.success) {
        await this.getProducts()
      }
      this.dialogDelete = false
    },
    async addNewProduct () {
      const formData = new FormData()
      for (const key in this.newProduct) {
        formData.append(key, this.newProduct[key])
      }
      const response = await this.$axios.post('/product/add', formData)
      if (response.data.success) {
        this.newProduct = {
          name: '',
          price: '',
          description: '',
          stock: '',
          category: '',
          image: null,
          brand: '',
          weight: '',
          dimensions: '',
          color: '',
          material: ''
        }
        this.dialogAdd = false
        await this.getProducts()
      }
    },
    updateProductMid (product) {
      this.selectedProduct = { ...product }
      this.dialogUpdate = true
    },
    async updateProduct () {
      const formData = new FormData()
      for (const key in this.selectedProduct) {
        formData.append(key, this.selectedProduct[key])
      }
      const response = await this.$axios.put(`/product/update/${this.selectedProduct.id}`, formData)
      if (response.data.success) {
        this.dialogUpdate = false
        await this.getProducts()
      }
    }
  }
}
</script>

<style scoped>
.add-btn-text {
  font-size: 16px;
  color: white;
}
.styled-table {
  border-radius: 12px;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
}
.styled-table .v-data-table-header {
  background-color: #3282B8;
  color: white;
}
.styled-table th {
  font-weight: bold;
}
.styled-table .v-data-table__cell {
  border-bottom: 1px solid #ddd;
}
.styled-table .v-data-table__row:hover {
  background-color: #f5f5f5;
}
.rounded-card {
  border-radius: 16px;
}
.custom-input .v-input__control {
  background-color: #F9FAFB !important;
  border-radius: 8px !important;
}
.custom-btn {
  color: white !important;
  border-radius: 24px !important;
  padding: 10px 20px;
}
.v-card-title {
  font-weight: bold;
}
</style>
