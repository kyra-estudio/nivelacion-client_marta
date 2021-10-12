<template>
  <div>
    <div class="row login_layout mt-5">
      <div class="col-md-4 mx-auto">
        <div class="card card-body">
          <h2 class="text-center">REGÍSTRATE</h2>

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
              v-model="password1"
              type="password"
              class="form-control"
              placeholder="Password"
            />
          </div>
          <div class="mb-3">
            <input
              v-model="password2"
              type="password"
              class="form-control"
              placeholder="Repite Password"
            />
          </div>
          <div class="d-grid gap-2 col-6 mx-auto">
            <button @click="onClick" type="button" class="btn btn-secondary">
              Entrar
            </button>
          </div>
          <div class="my-4 text-center">
            <p class="fw-bold">Ya estás registrado?</p>
            <a href="/sign-in" class="link-secondary">LOGIN</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
body {
  margin: 0;
  padding: 0;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background: #16bffd; /* fallback for old browsers */
  background: -webkit-linear-gradient(
    to right,
    #cb3066,
    #16bffd
  ); /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(
    to right,
    #cb3066,
    #16bffd
  ); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  min-height: 100vh;
}
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
      password1: '',
      password2: '',
    }
  },
  methods: {
    async onClick() {
      if (
        !this.email ||
        !this.password1 ||
        !this.password2 ||
        this.password1 !== this.password2
      ) {
        alert('El email o password no son correctos!!')
        return
      }

      //TODO
      try {
        const url = 'http://localhost:4500/api/user/sign-up'
        const body = {
          email: this.email,
          password1: this.password1,
          password2: this.password2,
        }

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
        } else {
          console.log({ data })
          this.$router.push('/sign-in')
        }
      } catch (err) {
        alert('Hubo un error')
      }
    },
  },
}
</script>
