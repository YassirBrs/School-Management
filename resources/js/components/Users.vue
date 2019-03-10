<template>
  <div class="container">
    <div class="row mt-5">
      <div class="col-md-12">
        <div class="card">
          <div class="card-header">
            <h3 class="card-title">Users Table</h3>

            <div class="card-tools">
              <button class="btn btn-success" data-toggle="modal" data-target="#addnew">
                Add
                <i class="fas fa-user-plus fa-fw"></i>
              </button>
            </div>
          </div>
          <!-- /.card-header -->
          <div class="card-body table-responsive p-0">
            <table class="table table-hover">
              <tbody>
                <tr>
                  <th>ID</th>
                  <th>Name</th>
                  <th>Email</th>
                  <th>Type</th>
                  <th>Ajouter le</th>
                  <th>Modifier</th>
                </tr>
                <tr v-for="user in users" :key="user.id">
                  <td>{{ user.id }}</td>
                  <td>{{ user.name|upText }}</td>
                  <td>{{ user.email }}</td>
                  <td>
                    <span class="tag tag-success">{{ user.type|upText }}</span>
                  </td>
                  <td>{{ user.created_at|myDate }}</td>
                  <td>
                    <a href>
                      <i class="fa fa-edit blue"></i>
                    </a>
                    /
                    <a href>
                      <i class="fa fa-trash red"></i>
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
    <div
      class="modal fade"
      id="addnew"
      tabindex="-1"
      role="dialog"
      aria-labelledby="addnewLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-dialog-centered" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="addnewLabel">Ajouter un Utilisateur</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <form @submit.prevent="createUser">
            <div class="modal-body">
              <div class="form-group">
                <input
                  v-model="form.name"
                  type="text"
                  placeholder="Name"
                  name="name"
                  class="form-control"
                  :class="{ 'is-invalid': form.errors.has('name') }"
                >
                <has-error :form="form" field="name"></has-error>
              </div>
              <div class="form-group">
                <input
                  v-model="form.email"
                  type="email"
                  placeholder="Email Address"
                  name="email"
                  class="form-control"
                  :class="{ 'is-invalid': form.errors.has('email') }"
                >
                <has-error :form="form" field="bio"></has-error>
              </div>
              <div class="form-group">
                <textarea
                  v-model="form.bio"
                  id="bio"
                  placeholder="Description (Facultatif)"
                  name="bio"
                  class="form-control"
                  :class="{ 'is-invalid': form.errors.has('bio') }"
                ></textarea>
                <has-error :form="form" field="bio"></has-error>
              </div>
              <div class="form-group">
                <select
                  v-model="form.type"
                  id="type"
                  name="type"
                  class="form-control"
                  :class="{ 'is-invalid': form.errors.has('type') }"
                >
                  <option value>Selectionner le Rôle</option>
                  <option value="admin">Admin</option>
                  <option value="user">Standard</option>
                  <option value="author">Auteur</option>
                </select>
                <has-error :form="form" field="password"></has-error>
              </div>
              <div class="form-group">
                <input
                  v-model="form.password"
                  type="password"
                  placeholder="Password"
                  name="password"
                  class="form-control"
                  :class="{ 'is-invalid': form.errors.has('password') }"
                >
                <has-error :form="form" field="password"></has-error>
              </div>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-danger" data-dismiss="modal">Annuler</button>
              <button type="submit" class="btn btn-primary">Créer</button>
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
      users: {},
      form: new Form({
        name: "",
        email: "",
        password: "",
        type: "",
        bio: "",
        photo: ""
      })
    };
  },
  methods: {
    loadUsers() {
      axios.get("api/user").then(({ data }) => (this.users = data.data));
    },
    createUser() {
      this.$Progress.start();
      this.form.post("api/user");
      $("#addnew").modal("hide");
      toast.fire({
        type: "success",
        title: "Utilisateur ajouter avec succès"
      });
      this.$Progress.finish();
    }
  },
  created() {
    this.loadUsers();
  },
  mounted() {
    console.log("Component mounted.");
  }
};
</script>
