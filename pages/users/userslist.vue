<template>
  <div>
    <div class="row login_layout mt-5">
      <div class="col-md-4 mx-auto">
        <div v-for="(user, index) in users" :key="index">
          <div class="mb-3">
              
            <input type="text" class="form-control" placeholder="{{ user }}" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData() {
    const url = 'http://localhost:4500/api/user/getAll'
    const res = await fetch(url)
    const data = await res.json()

    return {
      users: data.usersList,
    }
  },
}
</script>
