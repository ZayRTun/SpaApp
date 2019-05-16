<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-12">
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">All Users Table</h3>

                        <div class="card-tools">
                            <!-- Button trigger modal -->
                            <button type="button" class="btn btn-success btn-sm" @click="newModal">
                                Add User
                                <i class="fas fa-user-plus"></i>
                            </button>
                        </div>
                    </div>
                <!-- /.card-header -->
                <div class="card-body table-responsive p-0">
                    <table class="table table-hover">
                        <thead>

                        </thead>
                        <tbody>

                            <tr>
                                <th>ID</th>
                                <th>Name</th>
                                <th>Email</th>
                                <th>Type</th>
                                <th>Created On</th>
                                <th>Edit</th>
                                <th>Delete</th>
                            </tr>

                            <tr v-for="user in users" :key="user.id">
                                <td>{{user.id}}</td>
                                <td>{{user.name}}</td>
                                <td>{{user.email}}</td>
                                <td>{{user.type | upText}}</td>
                                <td>{{user.created_at | myDate}}</td>
                                <td>
                                    <a href="#" @click="editModal(user)" class="">Edit
                                    <i class="fas fa-edit"></i>
                                    </a>
                                </td>
                                <td>
                                    <a href="#" @click="deleteUser(user.id)" class="">Delete
                                    <i class="fas fa-trash-alt"></i>
                                    </a>
                                </td>
                            </tr>

                        </tbody>
                    </table>
                </div>
                <!-- /.card-body -->
                </div>
                <!-- /.card -->
            </div>
        </div>
        <!-- Modal -->
        <div class="modal fade" id="addUser" tabindex="-1" role="dialog" aria-labelledby="addUserTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" v-show="!editMode" id="addUserTitle">Add new user</h5>
                        <h5 class="modal-title" v-show="editMode" id="addUserTitle">Edit user info</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                        </button>
                    </div>

                    <form @submit.prevent="editMode ? updateUser() : createUser()" method="post">

                        <div class="modal-body">
                            <div class="form-group">
                                <input v-model="form.name" type="text" name="name" placeholder="Name"
                                class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
                                <has-error :form="form" field="name"></has-error>
                            </div>

                            <div class="form-group">
                                <input v-model="form.email" type="email" name="email" placeholder="Email"
                                class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
                                <has-error :form="form" field="email"></has-error>
                            </div>

                            <div class="form-group">
                                <select name="type" v-model="form.type" id="type" class="form-control" :class="{'is-invalid': form.errors.has('type')}">
                                    <option value="">Select User Role</option>
                                    <option value="admin">Admin</option>
                                    <option value="user">User</option>
                                </select>
                                <has-error :form="form" field="type"></has-error>
                            </div>

                            <div class="form-group">
                                <input v-model="form.password" type="password" name="password" placeholder="Password"
                                class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
                                <has-error :form="form" field="password"></has-error>
                            </div>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
                            <button type="submit" v-show="!editMode" class="btn btn-primary">Create</button>
                            <button type="submit" v-show="editMode" class="btn btn-success">Update</button>
                        </div>

                    </form>

                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      editMode: false,

      users: {},

      form: new Form({
        id: "",
        name: "",
        email: "",
        password: "",
        type: "",
        photo: ""
      })
    };
  },

  methods: {
    updateUser() {
      this.$Progress.start();
      this.form
        .put("api/user/" + this.form.id)
        .then(() => {
          // success
          $("#addUser").modal("hide");
          swal("Updated!", "The user was successfully updated.", "success");
          this.$Progress.finish();
          Fire.$emit("AfterCreated");
        })
        .catch(() => {
          // fail
          this.$Progress.fail();
        });
    },
    editModal(user) {
      this.editMode = true;
      this.form.reset();
      $("#addUser").modal("show");
      this.form.fill(user);
    },
    newModal() {
      this.editMode = false;
      this.form.reset();
      $("#addUser").modal("show");
    },
    deleteUser(id) {
      swal({
        title: "Are you sure?",
        text: "You won't be able to revert this!",
        type: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Yes, delete it!"
      }).then(result => {
        if (result.value) {
          this.form
            .delete("api/user/" + id)
            .then(() => {
              if (result.value) {
                swal("Deleted!", "Your file has been deleted.", "success");
              }
              Fire.$emit("AfterCreated");
            })
            .catch(() => {
              swal("Failed!", "There was somethind wrong.", "warning");
            });
        }
      });
    },

    loadUsers() {
      axios.get("api/user").then(({ data }) => (this.users = data.data));
    },

    createUser() {
      this.$Progress.start();
      this.form
        .post("api/user")
        .then(() => {
          Fire.$emit("AfterCreated");
          $("#addUser").modal("hide");
          toast({
            type: "success",
            title: "User created successfully"
          });
          this.$Progress.finish();
        })
        .catch(() => {
          this.$Progress.fail();
        });
    }
  },

  created() {
    this.loadUsers();
    Fire.$on("AfterCreated", () => this.loadUsers());
  }
};
</script>
