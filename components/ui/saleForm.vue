<template>
  <div>
    <!-- Invoice Summary -->
    <v-card class="mb-4" outlined>
      <v-row>
        <v-col cols="4">
          <v-img :src="selectedProduct?.image" aspect-ratio="1.7" />
        </v-col>
        <v-col cols="8">
          <v-card-title>{{ selectedProduct?.name || 'Product Name' }}</v-card-title>
          <v-card-subtitle>
            {{ selectedProduct?.brand || 'Brand' }} - {{ selectedProduct?.category || 'Category' }}
          </v-card-subtitle>
          <v-card-text>
            <p>Price: ${{ selectedProduct?.price || '0' }}</p>
            <p>Stock: {{ selectedProduct?.stock || 'N/A' }}</p>
            <p>Weight: {{ selectedProduct?.weight || 'N/A' }} kg</p>
            <p>Dimensions: {{ selectedProduct?.dimensions || 'N/A' }}</p>
            <p>Color: {{ selectedProduct?.color || 'N/A' }}</p>
            <p>Material: {{ selectedProduct?.material || 'N/A' }}</p>
          </v-card-text>
        </v-col>
      </v-row>
    </v-card>

    <!-- Invoice Form -->
    <v-dialog v-model="dialog" max-width="800">
      <v-card>
        <v-card-title class="text-h5">
          Invoice Form
        </v-card-title>
        <v-card-text>
          <v-stepper v-model="step">
            <!-- Step 1: Client Info -->
            <v-stepper-step :complete="step > 1" step="1">
              Client Info
            </v-stepper-step>
            <v-stepper-content step="1">
              <v-container>
                <v-row>
                  <v-col cols="6">
                    <v-select
                      v-model="selectedClient"
                      label="Select Client"
                      :items="clients"
                      item-text="name"
                      item-value="id"
                      outlined
                      required
                    />
                  </v-col>
                  <v-col cols="6">
                    <v-text-field
                      label="Phone"
                      :value="selectedClientData?.phone"
                      readonly
                      outlined
                    />
                  </v-col>
                  <v-col cols="12">
                    <v-text-field
                      label="Address"
                      :value="selectedClientData?.address"
                      readonly
                      outlined
                    />
                  </v-col>
                  <v-col cols="6">
                    <v-text-field
                      label="City"
                      :value="selectedClientData?.city"
                      readonly
                      outlined
                    />
                  </v-col>
                </v-row>
                <v-btn color="primary" @click="step = 2">
                  Next
                </v-btn>
              </v-container>
            </v-stepper-content>

            <!-- Step 2: Product Info -->
            <v-stepper-step :complete="step > 2" step="2">
              Product Info
            </v-stepper-step>
            <v-stepper-content step="2">
              <v-container>
                <v-row>
                  <v-col cols="6">
                    <v-text-field
                      v-model="transaction.quantity"
                      label="Quantity"
                      outlined
                      required
                    />
                  </v-col>
                  <v-col cols="6">
                    <v-text-field
                      v-model="transaction.totalPrice"
                      label="Total Price"
                      readonly
                      outlined
                      required
                    />
                  </v-col>
                </v-row>
                <v-btn color="primary" @click="step = 3">
                  Next
                </v-btn>
                <v-btn text @click="step = 1">
                  Back
                </v-btn>
              </v-container>
            </v-stepper-content>

            <!-- Step 3: Payment Method -->
            <v-stepper-step step="3">
              Payment Method
            </v-stepper-step>
            <v-stepper-content step="3">
              <v-container>
                <v-row>
                  <v-col cols="12">
                    <v-select
                      v-model="transaction.paymentMethod"
                      label="Payment Method"
                      :items="['Credit Card', 'Debit Card', 'PayPal', 'Bank Transfer']"
                      outlined
                      required
                    />
                  </v-col>
                </v-row>
                <v-btn color="primary" @click="step = 4">
                  Next
                </v-btn>
                <v-btn text @click="step = 2">
                  Back
                </v-btn>
              </v-container>
            </v-stepper-content>

            <!-- Step 4: Confirmation -->
            <v-stepper-step step="4">
              Confirmation
            </v-stepper-step>
            <v-stepper-content step="4">
              <v-container>
                <v-row>
                  <v-col cols="12">
                    <p>
                      <strong>Total Amount:</strong>
                      ${{ calculateTotalAmount() }}
                    </p>
                  </v-col>
                  <v-col cols="12">
                    <v-checkbox
                      v-model="transaction.agreeTerms"
                      label="I agree to the terms and conditions."
                      required
                    />
                  </v-col>
                </v-row>
                <v-btn color="success" @click="submitInvoice">
                  Confirm
                </v-btn>
                <v-btn text @click="step = 3">
                  Back
                </v-btn>
              </v-container>
            </v-stepper-content>
          </v-stepper>
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data () {
    return {
      dialog: true,
      step: 1,
      clients: [],
      selectedClient: null,
      selectedClientData: null,
      selectedProduct: null, // Data of the selected product
      transaction: {
        quantity: 1,
        totalPrice: 0,
        paymentMethod: '',
        agreeTerms: false
      }
    }
  },
  watch: {
    selectedClient: 'fetchSelectedClient',
    'transaction.quantity': 'calculateTotalPrice'
  },
  async mounted () {
    await this.fetchClients()
    const productId = this.$route.query.productId
    if (productId) {
      this.selectedProduct = await this.$axios.get(`/product/id/${productId}`).then(res => res.data.product)
      this.calculateTotalPrice()
    }
  },
  methods: {
    async fetchClients () {
      const response = await this.$axios.get('/client/all')
      this.clients = response.data.clients
    },
    async fetchSelectedClient () {
      if (this.selectedClient) {
        const response = await this.$axios.get(`/client/id/${this.selectedClient}`)
        this.selectedClientData = response.data.client
      }
    },
    calculateTotalPrice () {
      this.transaction.totalPrice = this.transaction.quantity * (this.selectedProduct?.price || 0)
    },
    calculateTotalAmount () {
      return this.transaction.totalPrice
    },
    async submitInvoice () {
      const invoiceData = {
        clientId: this.selectedClient,
        productId: this.selectedProduct.id,
        quantity: this.transaction.quantity,
        totalPrice: this.transaction.totalPrice,
        date: new Date().toISOString().split('T')[0],
        paymentMethod: this.transaction.paymentMethod,
        totalAmount: this.calculateTotalAmount(),
        transactionStatus: 'completed'
      }
      await this.$axios.post('/transaction/add', invoiceData)
      this.dialog = false
      this.$emit('alert', 'Invoice completed successfully!', 'success')
    }
  }
}
</script>
