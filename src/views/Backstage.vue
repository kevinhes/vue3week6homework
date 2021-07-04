<template>
    <div>
       <div class="container">
        <div class="row justify-content-center">
          <h1 class="h3 mb-3 font-weight-normal text-center mt-5">
            請先登入
          </h1>
          <div class="col-8">
            <Form ref="form" id="form" class="form-signin" v-slot="{ errors }" @submit="login">
              <div class="form-floating mb-3">
                <Field id="username" name="email" type="email" class="form-control" rules="email|required"
                      :class="{ 'is-invalid': errors['email'] }" placeholder="name@example.com"
                      v-model="user.username" required></Field>
                <error-message name="email" class="invalid-feedback"></error-message>
                <label for="username">Email address</label>
              </div>
              <div class="form-floating">
                <Field id="password" name="密碼" type="password" class="form-control" rules="email|required"
                      :class="{ 'is-invalid': errors['email'] }" placeholder="Password"
                      v-model="user.password" required></Field>
                <error-message name="密碼" class="invalid-feedback"></error-message>
                <label for="password">Password</label>
              </div>
              <button class="btn btn-lg btn-primary w-100 mt-3" type="submit">
                登入
              </button>
            </Form>
          </div>
        </div>
        <p class="mt-5 mb-3 text-muted text-center">
          &copy; 2021~∞ - 六角學院
        </p>
      </div>
    </div>
</template>

<script>
export default {
  data () {
    return {
      user: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    login () {
      const url = `${process.env.VUE_APP_API}admin/signin`
      this.$http.post(url, this.user)
        .then((res) => {
          if (res.data.success) {
            const token = res.data.token
            const expired = res.data.expired
            document.cookie = `hexToken=${token};expires=${new Date(expired)}; path=/`
            window.location = 'product.html'
          } else {
            alert(res.data.message)
          }
        }).catch((error) => {
          console.log(error)
        })
    }
  }
}
</script>
