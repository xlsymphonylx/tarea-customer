<template>
    <div>
        <div>
            <div class="h4 text-weight-bold">
                {{
                    isEditCustomer ? "Editando Cliente" : "Nuevo Cliente"
                }}
            </div>
            <div class="separator w-100 border-bottom"></div>
            <div class="form-group">
                <label for="name">Nombre Cliente</label>
                <input
                    type="text"
                    class="form-control"
                    id="name"
                    v-model="name"
                />
            </div>
            <div class="form-group">
                <label for="address">Direccion Cliente</label>
                <input
                    type="text"
                    class="form-control"
                    id="address"
                    v-model="address"
                />
            </div>
            <div class="form-group">
                <label for="phone_number">Telefono Cliente</label>
                <input
                    type="text"
                    class="form-control"
                    id="phone_number"
                    v-model="phone_number"
                />
            </div>
            <div class="text-center">
                <button
                    class="btn btn-success my-2"
                    @click="saveHandler"
                    v-if="!isEditCustomer"
                >
                    Guardar
                </button>
                <button
                    class="btn btn-success my-2"
                    @click="updateHandler"
                    v-else
                >
                    Actualizar
                </button>
                <button class="btn btn-danger my-2" @click="cancelHandler">
                    Cancelar
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data: () => ({
        name: "",
        address: "",
        phone_number: "",
    }),
    props: {
        customer: {
            type: Object,
            default: {},
        },
    },
    computed: {
        isEditCustomer() {
            return !!this.customer && Object.keys(this.customer).length !== 0;
        },
    },
    methods: {
        saveHandler() {
            this.$emit("save", {
                name: this.name,
                address: this.address,
                phone_number: this.phone_number,
            });
        },
        updateHandler() {
            this.$emit("update", {
                id: this.customer.id,
                name: this.name,
                address: this.address,
                phone_number: this.phone_number,
            });
        },
        updateCustomerInfo() {
            this.name = this.customer.name;
            this.phone_number = this.customer.phone_number;
            this.address = this.customer.address;
        },
        cleanCustomerInfo() {
            this.name = "";
            this.phone_number = "";
            this.address = "";
        },
        cancelHandler() {
            this.$emit("cancel");
        },
        cancelEditHandler() {
            this.$emit("cancelEdit");
        },
    },
};
</script>

<style></style>
