<template>
  <div>
    <div class="row login_layout mt-5">
      <div class="col-md-4 mx-auto">
        <div class="card card-body">
          <h2 class="text-center">Login</h2>

          <div class="mb-3">
            <input
              v-model="email"
              type="email"
              class="form-control"
              placeholder="email"
            />
          </div>
          <div class="mb-3">
            <input
              v-model="password"
              type="password"
              class="form-control"
              placeholder="Password"
            />
          </div>          
          <div class="d-grid gap-2 col-6 mx-auto">
            <button @click="onClick" type="button" class="btn btn-secondary">
              Entrar
            </button>
          </div>
          <div class="my-4 text-center">
            <p class="fw-bold">Aún no estás registrado??</p>
            <a href="/sign-up" class="link-secondary">REGÍSTRATE</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>

.card-body {
  background-color: antiquewhite;
}
.btn {
  margin-top: 25px;
}
</style>

<script>
export default {
  data() {
    return {
      email: '',
      password: '',
    }
  },
  methods: {
    async onClick() {
      const url = 'http://localhost:4500/api/user/sign-in'

      const body = {
        email: this.email,
        password: this.password,
      }

      try {
        const res = await fetch(url, {
          method: 'post',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(body),
        })

        const data = await res.json()
        if (data.error) {
          alert(data.error)
          return
        }

        console.log({ data })

        //Almacenar datos en localStorage:

        window.localStorage.setItem('token', data.token)
        window.localStorage.setItem('userAdmin', data.user.admin)
        window.localStorage.setItem('userId', data.user._id)

        //TODO:
        this.$router.push('/users/userslist')
      } catch (err) {
        
      }
    },
    
  },
}
</script>
