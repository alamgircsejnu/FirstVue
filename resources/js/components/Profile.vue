<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12">
                <div class="card card-widget widget-user">
                    <!-- Add the bg color to the header using any of the bg-* classes -->
                    <div class="widget-user-header text-white"
                         style="background: url('./img/user-cover.png') center center;">
                        <h3 class="widget-user-username text-right">Elizabeth Pierce</h3>
                        <h5 class="widget-user-desc text-right">Web Designer</h5>
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
                <div class="card">
                    <div class="card-header p-2">
                        <ul class="nav nav-pills">
                            <li class="nav-item"><a class="nav-link active" href="#activity"
                                                    data-toggle="tab">Activity</a></li>
                            <li class="nav-item"><a class="nav-link" href="#settings" data-toggle="tab">Settings</a>
                            </li>
                        </ul>
                    </div><!-- /.card-header -->
                    <div class="card-body">
                        <div class="tab-content">
                            <div class="tab-pane active" id="activity">

                            </div>
                            <div class="tab-pane" id="settings">
                                <form class="form-horizontal">
                                    <div class="form-group row">
                                        <label for="name" class="col-sm-2 col-form-label">Name</label>
                                        <div class="col-sm-10">
                                            <input type="text" name="name" v-model="form.name" class="form-control"
                                                   id="name" placeholder="Name">
                                        </div>
                                    </div>
                                    <div class="form-group row">
                                        <label for="email" class="col-sm-2 col-form-label">Email</label>
                                        <div class="col-sm-10">
                                            <input type="email" name="email" v-model="form.email" class="form-control"
                                                   id="email" placeholder="Email">
                                        </div>
                                    </div>
                                    <div class="form-group row">
                                        <label for="inputExperience" class="col-sm-2 col-form-label">Experience</label>
                                        <div class="col-sm-10">
                                            <textarea v-model="form.experience" class="form-control" id="inputExperience"
                                                      placeholder="Experience"></textarea>
                                        </div>
                                    </div>
                                    <div class="form-group row">
                                        <label for="inputSkills" class="col-sm-2 col-form-label">Skills</label>
                                        <div class="col-sm-10">
                                            <input type="text" v-model="form.skills" class="form-control" id="inputSkills"
                                                   placeholder="Skills (Comma separated)">
                                        </div>
                                    </div>
                                    <div class="form-group row">
                                        <label for="photo" class="col-sm-2 col-form-label">Profile Photo</label>
                                        <div class="col-sm-10">
                                            <input type="file" @change="updateProfilePhoto" class="form-control"
                                                 name="photo"  id="photo" placeholder="Profile Photo">
                                        </div>
                                    </div>
                                    <div class="form-group row">
                                        <label for="inputPassword" class="col-sm-2 col-form-label">Password</label>
                                        <div class="col-sm-10">
                                            <input type="password" v-model="form.password" class="form-control" id="inputPassword"
                                                   placeholder="Password (Leave empty if not changing)">
                                        </div>
                                    </div>
                                    <div class="form-group row mt-3">
                                        <div class="offset-sm-2 col-sm-10">
                                            <button type="submit" @click.prevent="updateProfile" class="btn btn-danger">Submit</button>
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

<script>
    export default {
        data() {
            return {
                form: new Form({
                    id: '',
                    name: '',
                    email: '',
                    password: '',
                    type: '',
                    experience: '',
                    skills: '',
                    photo: '',
                })
            }
        },
        methods: {
            getProfilePhoto(){
                let photo = (this.form.photo.length > 200) ? this.form.photo : "img/profile/"+ this.form.photo ;
                return photo;
            },
            updateProfile(){
               this.form.put('api/profile')
                   .then(() => {

                   })
                   .catch(() => {

                   });
            },
            updateProfilePhoto(element) {
                var file = element.target.files[0];
                var reader = new FileReader();

                if (file['size'] < 2111775) {
                    reader.onloadend = (file) => {
                        // console.log('RESULT', reader.result)
                        this.form.photo = reader.result;
                    }
                    reader.readAsDataURL(file);
                }else {
                    Swal.fire({
                        type:'error',
                        title: 'Oops...',
                        text: 'You have uploaded a file more than 2M'
                    });
                }

            }
        },
        mounted() {
            console.log('Component mounted.')
        },
        created() {
            axios.get('api/profile').then(({data}) => (this.form.fill(data)));
        }
    }
</script>
