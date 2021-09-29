<template>
  <div>
    <div class="row login_layout mt-5">
      <h2 class="h3 mt-2 text-center font-weight-normal">Sign-in</h2>

      <div class="col-md-4 mx-auto">
        <div class="mb-3">
          <input v-model="email" type="email" class="form-control"  placeholder="email"/>
        </div>
        <div class="mb-3">
          <input v-model="password" type="password" class="form-control" placeholder="Password"/>
        </div>
        <div class="d-grid gap-2 col-6 mx-auto">
          <button @click="onClick"  type="button" class="btn btn-secondary">Entrar</button>
        </div> 
        <div class= "my-4 text-center" >
          <a href="/sign-up">Sign-up</a>
        </div>
   </div>   
    </div>
  </div>
</template>

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

        //Almacenar token:
        const payload = {
          token: data.token,
        }
        window.localStorage.setItem('token', JSON.stringify(payload))

        //TODO:
        this.$router.push('/users/userslist')
      } catch (err) {}
    },
  },
}
</script>
