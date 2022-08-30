<template>
    <div class="m-5">
        <div class="h1 text-weight-bold">CRUD en Laravel y VUEjs</div>
        <div class="separator w-100 border-bottom"></div>
        <div id="table-container">
            <div class="text-end my-2">
                <button class="btn btn-success" @click="showModal">
                    Nuevo Cliente
                </button>
            </div>
            <table class="table table-striped table-bordered m-auto">
                <thead class="thead-dark">
                    <tr>
                        <th colspan="5">
                            <h5><center>CLIENTES</center></h5>
                        </th>
                    </tr>
                    <tr>
                        <th scope="col"><center>ID</center></th>
                        <th scope="col"><center>NOMBRE</center></th>
                        <th scope="col"><center>DIRECCION</center></th>
                        <th scope="col"><center>TELÉFONO</center></th>
                        <th scope="col"><center>ACCIONES</center></th>
                    </tr>
                </thead>
                <tbody id="tabla">
                    <tr v-for="customer in customers" :key="customer.id">
                        <td>{{ customer.id }}</td>
                        <td>{{ customer.name }}</td>
                        <td>{{ customer.address }}</td>
                        <td>{{ customer.phone_number }}</td>
                        <td class="text-center">
                            <button
                                class="btn btn-warning my-2"
                                @click="showEditForm(customer.id)"
                            >
                                Editar
                            </button>
                            <button
                                class="btn btn-danger my-2"
                                @click="deleteCustomer(customer.id)"
                            >
                                Eliminar
                            </button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <modal ref="commonModal">
            <customer-form
                ref="customerForm"
                @save="createCustomer"
                @cancel="closeModal"
                @update="updateCustomer"
                :customer="customer"
            ></customer-form>
        </modal>
    </div>
</template>

<script>
import Modal from "../components/Modal.vue";
import CustomerForm from "../components/customer/Form.vue";
import customerService from "../services/customerService";
export default {
    name: "customer-index",
    components: { Modal, CustomerForm },
    data: () => ({
        customers: [],
        customer: null,
    }),
    created() {
        this.getCustomers();
    },
    methods: {
        showModal() {
            this.$refs.commonModal.showModal();
        },
        async showEditForm(id) {
            await this.getCustomer(id);
            this.$refs.customerForm.updateCustomerInfo();
            this.showModal();
        },
        async getCustomers() {
            try {
                const response = await customerService.getCustomers();
                const data = response.data;
                this.customers = data;
            } catch (error) {}
        },
        async getCustomer(id) {
            try {
                const response = await customerService.getCustomer(id);
                const data = response.data;
                this.customer = {
                    id: data.id,
                    name: data.name,
                    address: data.address,
                    phone_number: data.phone_number,
                };
            } catch (error) {}
        },
        async createCustomer(customer) {
            try {
                await customerService.createCustomer(customer);
                this.getCustomers();
                this.closeModal();
            } catch (error) {}
        },
        async updateCustomer(customer) {
            try {
                const tempCustomer = customer;
                const idCustomer = tempCustomer.id;
                delete tempCustomer.id;
                await customerService.updateCustomer(idCustomer, tempCustomer);
                this.getCustomers();
                this.closeModal();
                this.customer = null;
            } catch (error) {}
        },
        async deleteCustomer(id) {
            try {
                await customerService.deleteCustomer(id);
                this.getCustomers();
            } catch (error) {}
        },

        closeModal() {
            this.$refs.commonModal.closeModal();
            this.$refs.customerForm.cleanCustomerInfo();
            this.customer = {};
        },
    },
};
</script>

<style></style>
