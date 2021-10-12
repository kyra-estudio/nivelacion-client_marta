<template>
  <div>
    <div class="row">
      <a
        class="fw-bold fs-3 text-white ms-3 mb-6 text-decoration-none"
        @click="logOut()"
        >Cerrar sesión</a
      >
    </div>
    <div class="row">
      <a
        class=" back fw-bold fs-3 text-white mb-6 text-decoration-none text-right"
        @click="volver()"
        >Volver</a
      >
    </div>

    <div class="row login_layout mt-5">
      <div class="col-md-4 mx-auto">
        <h1 class="text-center text-white mb-5">Chat con {{ userName }}</h1>
        <div>
          <div v-for="message in messages" :key="message._id">
            <div class="mb-3">
              <div v-if="message.userFrom == userFromId">
                <div class="card-body from">
                  <p class="fs-2 text-right fw-blod">{{ message.text }}</p>
                </div>
              </div>
              <div v-else>
                <div class="card-body to">
                  <p class="fs-2 text-left fw-blod">{{ message.text }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- <form class="row g-3"> -->
        <div class="row g-2">
          <div class="col-10">
            <input type="text" class="form-control" v-model="message" />
          </div>
          <div class="col-auto">
            <button
              type="button"
              class="btn-send btn-secundary mb-3"
              @click="send()"
            >
              Enviar
            </button>
          </div>
        </div>
        <!-- </form> -->
      </div>
    </div>
  </div>
</template>

<style>
.from {
  background-color: darksalmon !important;
  border-radius: 50px !important;
}
.to {
  background-color: darkseagreen !important ;
  border-radius: 50px !important;
}
.back{
    margin-left: -250px ;
}

</style>

<script>
export default {
  data() {
    return {
      userFromId: '',
      userToId: '',
      userName: '',
      messages: [],
    }
  },
  async beforeMount() {
    const userLogId = window.localStorage.getItem('userId')
    const userChatId = window.localStorage.getItem('userTo')
    const userChatName = window.localStorage.getItem('userToName')

    this.userFromId = userLogId
    this.userToId = userChatId
    this.userName = userChatName

    await this.getAllMessages()
  },
  methods: {
    async getAllMessages() {
      const url = 'http://localhost:4500/api/message/chat'

      const body = {
        userFromId: this.userFromId,
        userToId: this.userToId,
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

        this.messages = data.messages
      } catch (err) {
        alert(err)
      }
    },
    async send() {
      const url = 'http://localhost:4500/api/message/create'

      const body = {
        userFromId: this.userFromId,
        userToId: this.userToId,
        text: this.message,
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

        this.message = ''
        await this.getAllMessages()
      } catch (err) {
        alert(err)
      }
    },
    logOut() {
      window.localStorage.clear()
      this.$router.push(`/sign-in`)
    },
    volver() {

      this.$router.push(`/users/userslist`)
    },
  },
}
</script>
