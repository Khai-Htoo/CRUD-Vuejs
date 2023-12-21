<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-8 offset-4 mb-3">
        <div class="">
          <button class="btn btn-primary btn-sm" @click="createBtn">
            <i class="fas fa-plus-circle"> C r e a t e</i>
          </button>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-md-4">
        <div class="card">
          <div class="card-header">
            <h5>{{ isEdit ? "update" : "Create" }}</h5>
          </div>
          <div class="card-body">
            <div class="form-group">
              <form @submit.prevent="isEdit ? update() : store()">
                <label>Name</label>
                <input
                  type="text"
                  class="form-control"
                  :class="{ 'is-invalid': productStore.errors.has('price') }"
                  v-model="productStore.name"
                />
                <strong
                  class="text-danger"
                  v-if="productStore.errors.has('name')"
                  v-html="productStore.errors.get('name')"
                />
                <br />
                <label>Price</label>
                <input
                  type="number"
                  class="form-control mt-3"
                  :class="{ 'is-invalid': productStore.errors.has('price') }"
                  v-model="productStore.price"
                />
                <strong
                  class="text-danger"
                  v-if="productStore.errors.has('price')"
                  v-html="productStore.errors.get('price')"
                />
                <br />
                <button class="btn btn-primary mt-3" type="submit">
                  <i class="fas fa-save mr-2" style="font-size: 1.2rem"></i>
                  Save
                </button>
              </form>
            </div>
          </div>
        </div>
      </div>
      <div class="col-md-8">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Id</th>
              <th>Name</th>
              <th>Price</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="p in product.data" :key="p.id">
              <td>{{ p.id }}</td>
              <td>{{ p.name }}</td>
              <td>{{ p.price }} $</td>
              <td class="d-flex gap-2">
                <div class="">
                  <button class="btn btn-sm btn-info" @click="edit(p)">
                    <i class="fas fa-edit"></i> <span>Edit</span>
                  </button>
                </div>
                <div class="">
                  <button
                    class="ml-3 btn btn-sm btn-danger"
                    @click="destroy(p.id)"
                  >
                    <i class="fas fa-trash-alt"></i> <span>Delete</span>
                  </button>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Form from "vform";


export default {
  data() {
    return {
      isEdit: false,
      product: {},
      productStore: new Form({
        id: "",
        name: "",
        price: "",
      }),
    };
  },
  methods: {
    view(page = 1) {
      axios
        .get(`http://127.0.0.1:8000/api/product?page=${page}`)
        .then((res) => (this.product = res.data))
        .catch((err) => {
          console.log(err.message);
        });
    },
    store() {
      this.productStore
        .post("http://127.0.0.1:8000/api/product")
        .then((res) => {
          this.view();
          this.productStore.reset();
          this.$swal({
            toast: true,
            position: "top-end",
            showConfirmButton: false,
            timer: 3000,
            timerProgressBar: true,
            icon: "success",
            text: "Successfully created!",
          });
        })
        .catch((err) => {
          console.log(err.response.data.message);
        });
    },

    destroy(id) {
      this.$swal({
        title: "Are you sure?",
        text: "You won't be able to revert this!",
        icon: "warning",
        showCancelButton: true,
        reverseButtons: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Delete",
      }).then((result) => {
        if (result.isConfirmed) {
          axios
            .delete(`http://127.0.0.1:8000/api/product/${id}`)
            .then((res) => {
              this.view();
            })
            .catch((err) => {
              console.log(err.message);
            });
          this.$swal("Deleted!", "Your file has been deleted.", "success");
          this.$swal({
            toast: true,
            position: "top-end",
            showConfirmButton: false,
            timer: 3000,
            timerProgressBar: true,
            icon: "success",
            text: "Successfully deleted!",
          });
        }
      });
    },

    update() {
      this.productStore
        .put(`http://127.0.0.1:8000/api/product/${this.productStore.id}`)
        .then((res) => {
          this.view();
          this.productStore.reset();
          this.$swal({
            toast: true,
            position: "top-end",
            showConfirmButton: false,
            timer: 3000,
            timerProgressBar: true,
            icon: "success",
            text: "Successfully updated!",
          });
        })
        .catch((err) => {
          console.log(err.response.data.message);
        });
    },
    edit(product) {
      this.productStore.clear();
      this.isEdit = true;
      (this.productStore.id = product.id),
        (this.productStore.name = product.name),
        (this.productStore.price = product.price);
    },
    createBtn() {
      this.productStore.clear();
      this.isEdit = false;
      this.productStore.reset();
    },
  },
  mounted() {
    this.view();
  },
};
</script>

<style lang="scss" scoped></style>
