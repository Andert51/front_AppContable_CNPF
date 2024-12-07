<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-select
          v-model="selectedClient"
          :items="clients"
          item-text="name"
          item-value="id"
          label="Seleccione un Cliente"
          outlined
          @change="fetchTransactions"
        />
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-data-table
          :headers="headers"
          :items="transactions"
          item-key="id"
          class="elevation-1"
        >
          <template #[`item.actions`]="{ item }">
            <v-btn icon @click="editTransaction(item)">
              <v-icon>mdi-pencil</v-icon>
            </v-btn>
            <v-btn icon @click="deleteTransaction(item.id)">
              <v-icon color="red">
                mdi-delete
              </v-icon>
            </v-btn>
            <v-btn icon @click="printInvoice(item)">
              <v-icon color="blue">
                mdi-printer
              </v-icon>
            </v-btn>
          </template>
        </v-data-table>
      </v-col>
    </v-row>

    <!-- Recent Transactions Table -->
    <v-row>
      <v-col cols="12">
        <v-data-table
          :headers="recentHeaders"
          :items="recentTransactions"
          item-key="id"
          class="elevation-1"
        >
          <template #[`item.actions`]="{ item }">
            <v-btn icon @click="editTransaction(item)">
              <v-icon>mdi-pencil</v-icon>
            </v-btn>
            <v-btn icon @click="deleteTransaction(item.id)">
              <v-icon color="red">
                mdi-delete
              </v-icon>
            </v-btn>
            <v-btn icon @click="printInvoice(item)">
              <v-icon color="blue">
                mdi-printer
              </v-icon>
            </v-btn>
          </template>
        </v-data-table>
      </v-col>
    </v-row>

    <!-- Edit Rental Dialog -->
    <v-dialog v-model="dialog" max-width="600">
      <v-card>
        <v-card-title>Edit Transaction</v-card-title>
        <v-card-text>
          <v-form>
            <v-row>
              <v-col cols="6">
                <v-text-field
                  v-model="selectedTransaction.date"
                  label="Fecha de Compra"
                  outlined
                  required
                />
              </v-col>
              <v-col cols="6">
                <v-text-field
                  v-model="selectedTransaction.quantity"
                  label="Cantidad"
                  outlined
                  required
                />
              </v-col>
              <v-col cols="12">
                <v-text-field
                  v-model="selectedTransaction.transactionStatus"
                  label="Estatus"
                  outlined
                  required
                />
              </v-col>
              <v-col cols="12">
                <v-text-field
                  v-model="selectedTransaction.paymentMethod"
                  label="Informacion de Pago"
                  outlined
                  required
                />
              </v-col>
            </v-row>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-btn color="blue darken-1" text @click="dialog = false">
            Cancelar
          </v-btn>
          <v-btn color="blue darken-1" text @click="updateTransaction">
            Guardar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- Invoice Template -->
    <div id="invoice" style="display: none; font-family: Arial, sans-serif; width: 800px; margin: 0 auto; padding: 20px; box-sizing: border-box; border: 1px solid #ccc; border-radius: 8px;">
      <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
        <div style="flex: 1;">
          <img src="path/to/company-logo.png" alt="Company Logo" style="width: 150px; height: auto;">
        </div>
        <div style="text-align: center; flex: 2;">
          <h2>INVOICE</h2>
        </div>
        <div style="flex: 1; text-align: right;">
          <h4>No. {{ selectedTransaction.id }}</h4>
        </div>
      </div>

      <div style="margin-bottom: 20px;">
        <div style="display: flex; gap: 10px; margin-bottom: 10px;">
          <div style="flex: 1;">
            <label>Contacto</label>
            <p><strong>{{ selectedClientData?.name }}</strong></p>
            <div>
              <p><strong>Client:</strong> {{ selectedClientData?.name }}</p>
              <p><strong>ID:</strong> {{ selectedClientData?.id }}</p>
              <p><strong>Teléfono:</strong> {{ selectedClientData?.phone }}</p>
              <p><strong>Dirección:</strong> {{ selectedClientData?.address }}</p>
              <p><strong>Ciudad:</strong> {{ selectedClientData?.city }}</p>
            </div>
          </div>
          <div style="flex: 1;">
            <label>Detalles de la Transacción</label>
            <p><strong>Fecha:</strong> {{ selectedTransaction.date }}</p>
            <p><strong>Producto ID:</strong> {{ selectedTransaction.productId }}</p>
            <p><strong>Cantidad:</strong> {{ selectedTransaction.quantity }}</p>
            <p><strong>Total Pagado:</strong> {{ selectedTransaction.totalAmount }}</p>
            <p><strong>Método de Pago:</strong> {{ selectedTransaction.paymentMethod }}</p>
            <p><strong>Estatus:</strong> {{ selectedTransaction.transactionStatus }}</p>
          </div>
        </div>
      </div>

      <div style="margin-bottom: 20px;">
        <h3>Detalles del Producto</h3>
        <table style="width: 100%; border-collapse: collapse;">
          <thead>
            <tr style="background-color: #f2f2f2;">
              <th style="padding: 8px; border: 1px solid #ccc;">
                Producto
              </th>
              <th style="padding: 8px; border: 1px solid #ccc;">
                Precio Unitario
              </th>
              <th style="padding: 8px; border: 1px solid #ccc;">
                Cantidad
              </th>
              <th style="padding: 8px; border: 1px solid #ccc;">
                Total
              </th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td style="padding: 8px; border: 1px solid #ccc;">
                {{ selectedProduct?.name }}
              </td>
              <td style="padding: 8px; border: 1px solid #ccc;">
                ${{ selectedProduct?.price }}
              </td>
              <td style="padding: 8px; border: 1px solid #ccc;">
                {{ selectedTransaction.quantity }}
              </td>
              <td style="padding: 8px; border: 1px solid #ccc;">
                ${{ selectedTransaction.totalPrice }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <div style="margin-top: 20px; text-align: right;">
        <h3>Total: ${{ selectedTransaction.totalAmount }}</h3>
      </div>
    </div>
  </v-container>
</template>

<script>
import JsPDF from 'jspdf'
import html2canvas from 'html2canvas'

export default {
  data () {
    return {
      clients: [],
      transactions: [],
      recentTransactions: [],
      selectedClient: null,
      selectedTransaction: {},
      selectedClientData: null,
      selectedProduct: null,
      dialog: false,
      headers: [
        { text: 'Produc ID', value: 'productId' },
        { text: 'Cleint ID', value: 'clientId' },
        { text: 'Fecha de Compra', value: 'date' },
        { text: 'Cantidad', value: 'quantity' },
        { text: 'Informacion de Pago', value: 'paymentMethod' },
        { text: 'Total Pagado', value: 'totalAmount' },
        { text: 'Status', value: 'transactionStatus' },
        { text: 'Actions', value: 'actions', sortable: false }
      ],
      recentHeaders: [
        { text: 'Produc ID', value: 'productId' },
        { text: 'Cleint ID', value: 'clientId' },
        { text: 'Fecha de Compra', value: 'date' },
        { text: 'Cantidad', value: 'quantity' },
        { text: 'Informacion de Pago', value: 'paymentMethod' },
        { text: 'Total Pagado', value: 'totalAmount' },
        { text: 'Status', value: 'transactionStatus' },
        { text: 'Actions', value: 'actions', sortable: false }
      ]
    }
  },
  async mounted () {
    await this.fetchClients()
    await this.fetchRecentTransactions()
  },
  methods: {
    async fetchClients () {
      const response = await this.$axios.get('/client/all')
      this.clients = response.data.clients
      // eslint-disable-next-line no-console
      console.log('Clients fetched:', response)
    },
    async fetchTransactions () {
      if (this.selectedClient) {
        const response = await this.$axios.get(`/transaction/client/${this.selectedClient}`)
        this.transactions = response.data.transactions
        // eslint-disable-next-line no-console
        console.log('Transactions fetched:', response)
      }
    },
    async fetchRecentTransactions () {
      const response = await this.$axios.get('/transaction/all')
      this.recentTransactions = response.data.transactions
      // eslint-disable-next-line no-console
      console.log('Recent transactions fetched:', response)
    },
    editTransaction (transaction) {
      this.selectedTransaction = { ...transaction }
      this.dialog = true
    },
    async updateTransaction () {
      const response = await this.$axios.put(`/transaction/update/${this.selectedTransaction.id}`, this.selectedTransaction)
      if (response.data.success) {
        this.dialog = false
        await this.fetchTransactions()
        await this.fetchRecentTransactions()
      }
    },
    async deleteTransaction (transactionId) {
      const response = await this.$axios.delete(`/transaction/delete/${transactionId}`)
      if (response.data.success) {
        await this.fetchTransactions()
        await this.fetchRecentTransactions()
      }
    },
    async printInvoice (transaction) {
      this.selectedTransaction = { ...transaction }
      this.selectedClientData = this.clients.find(client => client.id === transaction.clientId)
      this.selectedProduct = await this.$axios.get(`/product/id/${transaction.productId}`).then(res => res.data.product)
      await this.$nextTick()
      const invoiceElement = document.getElementById('invoice')
      invoiceElement.style.display = 'block'
      html2canvas(invoiceElement, { scale: 2 }).then((canvas) => {
        const imgData = canvas.toDataURL('image/png')
        const pdf = new JsPDF()
        pdf.addImage(imgData, 'PNG', 10, 10, 190, 0)
        pdf.save(`${this.selectedClientData.name}_Factura.pdf`)
        invoiceElement.style.display = 'none'
      }).catch((error) => {
        // eslint-disable-next-line no-console
        console.error('Error generating PDF:', error)
        invoiceElement.style.display = 'none'
      })
    }
  }
}
</script>

<style scoped>
</style>
