<template>
  <div>
    <a
      class="fw-bold fs-3 text-white ms-3 mb-6 text-decoration-none"
      @click="logOut()"
      >Cerrar sesión</a
    >
    
    <div class="row login_layout mt-5">
      <div class="col-md-4 mx-auto">
        <div class="card">
          <div v-for="user in users" :key="user._id">
            <div class="mb-3">
              <div class="card-body">
                <p class="fs-2 fw-blod" >{{ user.email }}</p>
                <div v-if="userAdmin > 0 && userId != user._id">
                  <div class="d-grid gap-2 col-6 mx-auto">
                  <button class="btn-delete" type="button" @click="removeUser(user._id)">
                    Borrar</button>
                    <button
                    class="btn"
                    type="button"
                    @click="getChat(user._id,user.email)"
                  >
                    Chat
                  </button>
                </div>
                 </div>
                 
                <div v-else>
                  <div class="d-grid gap-2 col-6 mx-auto">
                  <button @click="getChat(user._id,user.email)" class="btn">Chat</button>
                </div>
                </div>
              </div>
            </div>
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
.btn-delete{
  background-color: blueviolet;
  color: darkgrey;
   border-radius: 50px !important
}
</style>

<script>
export default {
  data() {
    return {
      users: [],
      userAdmin: 0,
      userId: '',
    }
  },
  async beforeMount() {
    const token = window.localStorage.getItem('token')
    const admin = window.localStorage.getItem('userAdmin')
    const userId = window.localStorage.getItem('userId')

    this.userId = userId

    if (admin === 'true') {
      this.userAdmin = 1
    } else {
      this.userAdmin = 0
    }

    await this.getAllUsers(token)
  },
  methods: {
    getChat(id, email) {
      window.localStorage.setItem('userTo', id)
      window.localStorage.setItem('userToName', email)
      this.$router.push(`/users/usersmessage`)
    },
    async getAllUsers(token) {
      try {
        const url = 'http://localhost:4500/api/user/getAll'
        const res = await fetch(url)
        const data = await res.json()

        if (data.error) {
          alert(data.error)
        }

        this.users = data.users
      } catch (err) {
        alert(err)
      }
    },
    async removeUser(id) {
      const url = `http://localhost:4500/api/user/${id}`

      await fetch(url, { method: 'delete' })
      await this.getAllUsers()
    },
    logOut() {
      window.localStorage.clear()
      this.$router.push(`/sign-in`)
    },
  },
}
</script>
