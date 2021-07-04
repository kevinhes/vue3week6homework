<template>
    <div>
    <div class="text-end pt-3">
         <button class="btn btn-outline-danger" type="button" @click="cleanCarts">清空購物車</button>
    </div>
        <table class="table align-middle">
            <thead>
              <tr>
                <th></th>
                <th>品名</th>
                <th style="width: 150px">數量/單位</th>
                <th>單價</th>
              </tr>
            </thead>
            <tbody>
              <template v-if="carts.carts">
                <tr v-for="item in carts.carts" :key="item">
                  <td>
                    <button type="button" class="btn btn-outline-danger btn-sm" @click="cleanOne(item)">
                      <i class="fas fa-spinner fa-pulse" v-if="loadingStatus.loadingItem === item.product_id"></i>
                      x
                    </button>
                  </td>
                  <td>
                    {{ item.product.title }}
                  </td>
                  <td>
                    <div class="input-group input-group-sm">
                      <div class="input-group mb-3">
                        <input
                             min="1"
                             type="number"
                             class="form-control"
                             v-model.number="item.qty"
                             @change="updateCart(item)"
                             :disabled="loadingStatus.loadingItem === item.id">
                        <span class="input-group-text" id="basic-addon2"></span>
                      </div>
                    </div>
                  </td>
                  <td class="text-end">
                    {{ item.total}}
                  </td>
                </tr>
              </template>
            </tbody>
            <tfoot>
              <tr>
                <td colspan="3" class="text-end">總計</td>
                <td class="text-end">{{ carts.total }}</td>
              </tr>
              <tr>
                <td colspan="3" class="text-end text-success">折扣價</td>
                <td class="text-end text-success">{{ carts.final_total }}</td>
              </tr>
            </tfoot>
        </table>
        <div class="my-5 row justify-content-center">
          <Form ref="form" class="col-md-6" v-slot="{ errors }" @submit="onSubmit">
            <div class="mb-3">
              <label for="email" class="form-label">Email</label>
              <Field id="email" name="email" type="email" class="form-control" rules="email|required"
                      :class="{ 'is-invalid': errors['email'] }" placeholder="請輸入 Email"
                      v-model="form.user.email"></Field>
              <error-message name="email" class="invalid-feedback"></error-message>
            </div>
            <div class="mb-3">
              <label for="name" class="form-label">收件人姓名</label>
              <Field id="name" name="姓名" type="text" class="form-control" :class="{ 'is-invalid': errors['姓名'] }"
                       placeholder="請輸入姓名" rules="required" v-model="form.user.name"></Field>
              <error-message name="姓名" class="invalid-feedback"></error-message>
            </div>

            <div class="mb-3">
              <label for="address" class="form-label">電話</label>
              <Field id="address" name="電話" type="text" class="form-control" :class="{ 'is-invalid': errors['電話'] }"
                       placeholder="請輸入電話" :rules="isPhone" v-model="form.user.tel"></Field>
              <error-message name="電話" class="invalid-feedback"></error-message>
            </div>
            <div class="mb-3">
              <label for="address" class="form-label">收件人地址</label>
              <Field id="address" name="地址" type="text" class="form-control" :class="{ 'is-invalid': errors['地址'] }"
                       placeholder="請輸入地址" rules="required" v-model="form.user.address"></Field>
              <error-message name="地址" class="invalid-feedback"></error-message>
            </div>
            <div class="mb-3">
              <label for="message" class="form-label">留言</label>
              <textarea id="message" class="form-control" cols="30" rows="10" v-model="message"></textarea>
            </div>
            <div class="text-end">
              <button type="submit" class="btn btn-danger">送出訂單</button>
            </div>
          </Form>
        </div>
    </div>
</template>

<script>
export default {
  data () {
    return {
      carts: {},
      loadingStatus: {
        loadingItem: ''
      },
      form: {
        user: {
          name: '',
          email: '',
          tel: '',
          address: ''
        }
      },
      message: ''
    }
  },
  methods: {
    getCartInfo () {
      const url = `${process.env.VUE_APP_API}/${process.env.VUE_APP_PATH}/cart`
      this.$http.get(url)
        .then((res) => {
          if (res.data.success) {
            this.carts = res.data.data
          } else { alert(res.data.message) }
        })
    },
    updateCart (item) {
      const cart = {
        product_id: item.product.id,
        qty: item.qty
      }
      const url = `${process.env.VUE_APP_API}/${process.env.VUE_APP_PATH}/cart/${item.id}`
      this.loadingStatus.loadingItem = item.id
      console.log(cart)
      this.$http.put(url, { data: cart })
        .then((res) => {
          if (res.data.success) {
            alert(res.data.message)
            this.loadingStatus.loadingItem = ''
            this.getCartInfo()
          }
        })
    },
    cleanCarts () {
      const url = `${process.env.VUE_APP_API}/${process.env.VUE_APP_PATH}/carts`
      this.$http.delete(url)
        .then((res) => {
          if (res.data.success) {
            alert(res.data.message)
            this.getCartInfo()
          }
        })
    },
    cleanOne (item) {
      const url = `${process.env.VUE_APP_API}/${process.env.VUE_APP_PATH}/cart/${item.id}`
      this.$http.delete(url)
        .then((res) => {
          if (res.data.success) {
            alert(res.data.message)
            this.getCartInfo()
          }
        })
    },
    isPhone (value) {
      const phoneNumber = /^(09)[0-9]{8}$/
      return phoneNumber.test(value) ? true : '需要正確的電話號碼'
    },
    onSubmit () {
      const user = this.form
      this.$http.post(`${this.apiUrl}/${this.apiPath}/order`, { data: user })
        .then((res) => {
          if (res.data.success) {
            alert(res.data.message)
          }
        })
    }
  },
  mounted () {
    this.getCartInfo()
  }
}
</script>
