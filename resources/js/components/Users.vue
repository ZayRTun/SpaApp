<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-12">
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">All Users Table</h3>

                        <div class="card-tools">
                            <!-- Button trigger modal -->
                            <button type="button" class="btn btn-success btn-sm" data-toggle="modal" data-target="#addUser">
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
                                <th>Edit</th>
                                <th>Delete</th>
                            </tr>

                            <tr>
                                <td>124</td>
                                <td>Zayar</td>
                                <td>zay.t@hotmail.com</td>
                                <td>Admin</td>
                                <td>
                                    <a href="#" class="">Edit
                                    <i class="fas fa-edit"></i>
                                    </a>
                                </td>
                                <td>
                                    <a href="#" class="">Delete
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
                    <h5 class="modal-title" id="addUserTitle">Create User</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                    </button>
                </div>
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
                        <input v-model="form.email" type="email" name="email" placeholder="Email"
                           class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
                        <has-error :form="form" field="email"></has-error>
                    </div>

                    <div class="form-group">
                        <select name="type" v-model="form.type" id="type" class="form-control" :class="{'is-invalid': form.errors.has('type')}">
                            <option value="">Select User Role</option>
                            <option value="admin">Admin</option>
                            <option value="user">Regular User</option>
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
                    <button type="button" class="btn btn-primary">Create</button>
                </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            form: new Form({
                name: '',
                email: '',
                password: '',
                type: '',
                bio: '',
                photo: ''
            })
        }
    },

    mounted() {
        console.log("Users mounted.");
    }
};
</script>
