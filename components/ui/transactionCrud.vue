<template>
  <div class="pa-3 ma-3">
    <v-row align="center" justify="center">
      <h1>Administracion de Transacciones</h1>
    </v-row>
    <v-row align="center" justify="end">
      <v-btn color="#3282B8" @click="dialogAdd = true">
        <v-icon color="white">
          mdi-plus
        </v-icon>
        <span style="text-transform: none;" class="add-btn-text">
          Agregar Nueva Transacción
        </span>
      </v-btn>
    </v-row>
    <v-row align="center" justify="center">
      <v-data-table
        :headers="headers"
        :items="transactions_data"
        item-key="id"
        class="styled-table"
      >
        <template #[`item.actions`]="{ item }">
          <v-tooltip bottom color="orange">
            <template #activator="{ on, attrs}">
              <v-btn
                icon
                color="warning"
                v-bind="attrs"
                @click="updateTransactionMid(item)"
                v-on="on"
              >
                <v-icon>
                  mdi-pencil-box-outline
                </v-icon>
              </v-btn>
            </template>
            <span>Actualizar Transacción: {{ item.id }}</span>
          </v-tooltip>
          |
          <v-tooltip bottom color="red">
            <template #activator="{ on, attrs}">
              <v-btn
                icon
                color="red"
                v-bind="attrs"
                @click="deleteTransactionMid(item.id)"
                v-on="on"
              >
                <v-icon>
                  mdi-trash-can
                </v-icon>
              </v-btn>
            </template>
            <span>Eliminar Transacción: {{ item.id }}</span>
          </v-tooltip>
        </template>
      </v-data-table>
    </v-row>
    <v-dialog v-model="dialogAdd" width="600">
      <v-card class="pa-5 rounded-card" width="600" color="white">
        <v-card-title class="text-h5 text-center mb-4" style="color: #1B262C;">
          Agregar Nueva Transacción
        </v-card-title>

        <v-card-text>
          <v-form ref="form">
            <v-row dense>
              <v-col cols="12" sm="6">
                <v-select
                  v-model="newTransaction.clientId"
                  :items="clients"
                  label="Cliente"
                  outlined
                  dense
                  class="custom-input"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-select
                  v-model="newTransaction.productId"
                  :items="products"
                  label="Producto"
                  outlined
                  dense
                  class="custom-input"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newTransaction.quantity"
                  label="Cantidad"
                  outlined
                  dense
                  class="custom-input"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="newTransaction.totalPrice"
                  label="Precio Total"
                  outlined
                  dense
                  class="custom-input"
                  required
                />
              </v-col>
              <v-col cols="12">
                <v-text-field
                  v-model="newTransaction.date"
                  label="Fecha"
                  type="date"
                  outlined
                  dense
                  class="custom-input"
                  required
                />
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>

        <v-card-actions class="justify-center mt-4">
          <v-btn color="grey darken-1" class="cancel-btn mx-2" @click="dialogAdd = false">
            Cancelar
          </v-btn>
          <v-btn color="blue darken-1" class="custom-btn mx-2" @click="addNewTransaction">
            Agregar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="dialogUpdate" width="600">
      <v-card class="pa-6 rounded-update-card" width="650" color="grey lighten-4">
        <v-card-title class="text-h5 text-center mb-5" style="color: #34495E;">
          Actualizar Transacción
        </v-card-title>

        <v-card-text>
          <v-form ref="form">
            <v-row dense>
              <v-col cols="12" sm="6">
                <v-select
                  v-model="selectedTransaction.clientId"
                  :items="clients"
                  label="Cliente"
                  outlined
                  dense
                  class="update-input"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-select
                  v-model="selectedTransaction.productId"
                  :items="products"
                  label="Producto"
                  outlined
                  dense
                  class="update-input"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="selectedTransaction.quantity"
                  label="Cantidad"
                  outlined
                  dense
                  class="update-input"
                  required
                />
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="selectedTransaction.totalPrice"
                  label="Precio Total"
                  outlined
                  dense
                  class="update-input"
                  required
                />
              </v-col>
              <v-col cols="12">
                <v-text-field
                  v-model="selectedTransaction.date"
                  label="Fecha"
                  type="date"
                  outlined
                  dense
                  class="update-input"
                  required
                />
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>

        <v-card-actions class="d-flex justify-center mt-4">
          <v-btn color="grey darken-1" class="cancel-btn mx-2" @click="dialogUpdate = false">
            Cancelar
          </v-btn>
          <v-btn color="teal darken-1" class="update-btn mx-2" @click="updateTransaction">
            Actualizar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog
      v-model="dialogDelete"
      width="400"
      persistent
    >
      <v-card>
        <v-card-title>
          <v-row align="center" justify="center" class="pa-2 ma-2">
            Eliminar Transacción
          </v-row>
        </v-card-title>
        <v-card-text>
          <v-row align="center" justify="center" class="pa-2 ma-2">
            Esta seguro de eliminar esta transacción?
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-row align="center" justify="center" class="pa-2 ma-2">
            <v-btn color="warning" @click="dialogDelete=false">
              <span style="text-transform: none;">
                Cancelar
              </span>
            </v-btn>
            <v-btn color="red" @click="deleteTransaction">
              <span style="text-transform: none;">
                Eliminar
              </span>
            </v-btn>
          </v-row>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data () {
    return {
      transactions_data: [],
      clients: [], // Assuming you have a list of clients
      products: [], // Assuming you have a list of products
      headers: [
        { text: 'Cliente', align: 'center', sortable: true, value: 'clientId' },
        { text: 'Producto', align: 'center', sortable: true, value: 'productId' },
        { text: 'Cantidad', align: 'center', sortable: true, value: 'quantity' },
        { text: 'Precio Total', align: 'center', sortable: true, value: 'totalPrice' },
        { text: 'Fecha', align: 'center', sortable: true, value: 'date' },
        { text: 'Acciones', align: 'center', sortable: false, value: 'actions' }
      ],
      dialogAdd: false,
      dialogUpdate: false,
      dialogDelete: false,
      newTransaction: {
        clientId: '',
        productId: '',
        quantity: '',
        totalPrice: '',
        date: ''
      },
      selectedTransaction: {
        id: '',
        clientId: '',
        productId: '',
        quantity: '',
        totalPrice: '',
        date: ''
      },
      id: null
    }
  },
  mounted () {
    this.getTransactions()
    this.getClients()
    this.getProducts()
  },
  methods: {
    async getTransactions () {
      const res = await this.$axios.get('/transaction/all')
      if (res && res.data && res.data.success) {
        this.transactions_data = res.data.transactions
      }
    },
    async getClients () {
      const res = await this.$axios.get('/client/all')
      if (res && res.data && res.data.success) {
        this.clients = res.data.clients.map(client => ({ text: client.username, value: client.id }))
      }
    },
    async getProducts () {
      const res = await this.$axios.get('/product/all')
      if (res && res.data && res.data.success) {
        this.products = res.data.products.map(product => ({ text: product.name, value: product.id }))
      }
    },
    deleteTransactionMid (idDel) {
      this.id = idDel
      this.dialogDelete = true
    },
    async deleteTransaction () {
      const res = await this.$axios.delete(`/transaction/delete/${this.id}`)
      if (res && res.data && res.data.success) {
        await this.getTransactions()
      }
      this.dialogDelete = false
    },
    async addNewTransaction () {
      const formData = new FormData()
      for (const key in this.newTransaction) {
        formData.append(key, this.newTransaction[key])
      }
      const response = await this.$axios.post('/transaction/add', formData)
      if (response.data.success) {
        this.newTransaction = {
          clientId: '',
          productId: '',
          quantity: '',
          totalPrice: '',
          date: ''
        }
        this.dialogAdd = false
        await this.getTransactions()
      }
    },
    updateTransactionMid (transaction) {
      this.selectedTransaction = { ...transaction }
      this.dialogUpdate = true
    },
    async updateTransaction () {
      const formData = new FormData()
      for (const key in this.selectedTransaction) {
        formData.append(key, this.selectedTransaction[key])
      }
      const response = await this.$axios.put(`/transaction/update/${this.selectedTransaction.id}`, formData)
      if (response.data.success) {
        this.dialogUpdate = false
        await this.getTransactions()
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
