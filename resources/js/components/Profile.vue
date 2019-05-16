<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card card-widget widget-user">

                    <!-- Add the bg color to the header using any of the bg-* classes -->
                    <div class="widget-user-header text-white" style="background: url('./images/bg1_640.jpg') center center;">
                        <h3 class="widget-user-username">Elizabeth Pierce</h3>
                        <h5 class="widget-user-desc">Web Designer</h5>
                    </div>
                    <div class="widget-user-image">
                        <img class="img-circle" :src="getProfilePhoto()" alt="User Avatar">
                    </div>
                    <div class="card-footer">
                        <div class="row">
                            <div class="col-sm-4 border-right">
                                <div class="description-block">
                                    <h5 class="description-header">3,200</h5>
                                    <span class="description-text">SALES</span>
                                </div>
                            <!-- /.description-block -->
                            </div>
                        <!-- /.col -->
                            <div class="col-sm-4 border-right">
                                <div class="description-block">
                                    <h5 class="description-header">13,000</h5>
                                    <span class="description-text">FOLLOWERS</span>
                                </div>
                            <!-- /.description-block -->
                            </div>
                        <!-- /.col -->
                            <div class="col-sm-4">
                                <div class="description-block">
                                    <h5 class="description-header">35</h5>
                                    <span class="description-text">PRODUCTS</span>
                                </div>
                            <!-- /.description-block -->
                            </div>
                        <!-- /.col -->
                        </div>
                    <!-- /.row -->
                    </div>
                </div>
            </div>
            <div class="col-md-12">
                <div class="card">
                    <div class="card-header p-2">
                        <ul class="nav nav-pills">
                            <li class="nav-item"><a class="nav-link active show" href="#activity" data-toggle="tab">Activity</a></li>
                            <li class="nav-item"><a class="nav-link" href="#settings" data-toggle="tab">Settings</a></li>
                        </ul>
                    </div><!-- /.card-header -->
                    <div class="card-body">

                        <div class="tab-content">
                            <div class="tab-pane active show" id="activity">

                            </div>

                            <div class="tab-pane" id="settings">
                                <form class="form-horizontal" enctype="multipart/form-data">

                                    <div class="form-group">
                                        <label for="inputName2" class="col-sm-2 control-label">Name</label>

                                        <div class="col-sm-12">
                                            <!-- <input type="text" class="form-control" id="inputName2" placeholder="Name" v-model="form.name"> -->

                                            <input v-model="form.name" type="text" name="name" placeholder="Name" class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
                                            <has-error :form="form" field="name"></has-error>
                                        </div>
                                    </div>

                                    <div class="form-group">
                                        <label for="inputEmail" class="col-sm-2 control-label">Email</label>

                                        <div class="col-sm-12">
                                            <input v-model="form.email" type="email" name="email" placeholder="Email" class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
                                            <has-error :form="form" field="email"></has-error>
                                        </div>
                                    </div>

                                    <div class="form-group">
                                        <label for="inputPassword" class="col-sm-2 control-label">Password</label>

                                        <div class="col-sm-12">
                                            <input v-model="form.password" type="password" name="password" placeholder="Password"
                                            class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
                                            <has-error :form="form" field="password"></has-error>
                                        </div>
                                    </div>

                                    <div class="form-group">
                                        <label for="userImage" class="col-sm-12 control-label">Select image to upload:</label>

                                        <div class="col-sm-12">
                                            <input type="file" name="userImage" id="userImage" @change="updateProfile">
                                        </div>
                                    </div>

                                    <div class="form-group">
                                        <div class="col-sm-offset-2 col-sm-12">
                                            <button @click.prevent="updateInfo" type="submit" class="btn btn-success">Update</button>
                                        </div>
                                    </div>
                                </form>
                            </div>
                        <!-- /.tab-pane -->
                        </div>
                    <!-- /.tab-content -->
                    </div><!-- /.card-body -->
                </div>
            </div>
        </div>
    </div>
</template>

<style type="text/css">

    .widget-user-header {
        background-position: center center;
        background-size: cover;
    }

</style>

<script>
    export default {
        data() {
            return {
                form: new Form({
                        id: "",
                        name: "",
                        email: "",
                        password: "",
                        type: "",
                        photo: ""
                      })
            }
        },
        methods: {
            getProfilePhoto() {
                return "./images/profile/" + this.form.photo;
            },
            updateInfo() {
                this.$Progress.start();
                this.form.put('api/profile')
                .then(() => {

                    this.$Progress.finish();
                })
                .catch(() => {

                    this.$Progress.fail();
                });
            },

            updateProfile(element) {
                if  (this.form.password == "") {
                    this.form.password = undefined;
                }
                let file = element.target.files[0];
                var reader = new FileReader();
                if (file['size'] < 2097152) {
                    reader.onloadend = (file) => {
                        // console.log('RESULT', reader.result);
                        this.form.photo = reader.result;
                    }
                    reader.readAsDataURL(file);
                } else {
                    swal({
                        type: 'error',
                        title: 'Oops...',
                        text: 'Maximum upload size is 2mb.',
                    })
                }
            }
        },
        mounted() {
            console.log('Profile mounted.')
        },
        created() {
            axios.get("api/profile")
            .then(({data}) => (this.form.fill(data)));
        }
    }
</script>
