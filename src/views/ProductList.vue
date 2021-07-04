<template>
  <div class="container">
        <div class="mt-4">
          <!-- 產品Modal -->
          <product-modal ref="userProductModal" :product="product" @add-cart="addCart"></product-modal>
          <table class="table align-middle">
            <thead>
              <tr>
                <th>圖片</th>
                <th>商品名稱</th>
                <th>價格</th>
                <th></th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item) in productsData" :key="item.id">
                <td style="width: 200px">
                  <div style="height: 100px; background-size: cover; background-position: center" :style="{ 'background-image' : `url(${item.imageUrl})` }"></div>
                </td>
                <td>
                  {{ item.title }}
                </td>
                <td>
                  <div v-if="item.price < item.origin_price">
                    <del class="h6" >原價 {{ item.price }} 元</del>
                    <div class="h5">現在只要 {{ item.origin_price }} 元</div>
                  </div>
                  <div class="h5" v-else>{{ item.price }} 元</div>
                </td>
                <td>
                  <div class="btn-group btn-group-sm">
                    <button type="button"
                            class="btn btn-outline-secondary"
                            @click="productModal(item)"
                            :disabled="loadingStatus.loadingItem === item.id">
                    <div class="spinner-border spinner-border-sm" role="status" v-if="loadingStatus.loadingItem === item.id">
                      <span class="visually-hidden">Loading...</span>
                    </div>
                      查看更多
                    </button>
                    <router-link :to="`/product/${item.id}`">
                    <button type="button"
                            class="btn btn-outline-warning"
                            :disabled="loadingStatus.loadingItem === item.id">
                    <div class="spinner-border spinner-border-sm" role="status" v-if="loadingStatus.loadingItem === item.id">
                      <span class="visually-hidden">Loading...</span>
                    </div>
                      查看更多(單一頁面)
                    </button>
                    </router-link>
                    <button type="button"
                            class="btn btn-outline-danger"
                            @click="addCart(item.id,qty)"
                            :disabled="loadingStatus.loadingItem === item.id">
                    <div class="spinner-border spinner-border-sm" role="status" v-if="loadingStatus.loadingItem === item.id">
                      <span class="visually-hidden">Loading...</span>
                    </div>
                      加到購物車
                    </button>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
          <!-- 購物車列表 -->
        </div>
      </div>
</template>

<script>
import ProductModal from '@/components/ProductModal.vue'
export default {
  components: {
    ProductModal
  },
  data () {
    return {
      productsData: {},
      product: {},
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
    getData (page = 1) {
      const url = `${process.env.VUE_APP_API}/${process.env.VUE_APP_PATH}/products?page=${page}`
      this.$http.get(url)
        .then((res) => {
          if (res.data.success) {
            console.log(res.data)
            this.productsData = res.data.products
          } else {
            alert(res.data.message)
          }
        })
    },
    productModal (item) {
      const url = `${process.env.VUE_APP_API}/${process.env.VUE_APP_PATH}/product/${item.id}`
      this.loadingStatus.loadingItem = item.id
      this.$http.get(url)
        .then((res) => {
          if (res.data.success) {
            this.$refs.userProductModal.openModal()
            this.loadingStatus.loadingItem = ''
            this.product = res.data.product
          } else {
            alert(res.data.message)
          }
        })
    },
    addCart (id, qty = 1) {
      const cart = {
        product_id: id,
        qty
      }
      const url = `${process.env.VUE_APP_API}/${process.env.VUE_APP_PATH}/cart`
      this.loadingStatus.loadingItem = id
      this.$http.post(url, { data: cart })
        .then((res) => {
          if (res.data.success) {
            alert(res.data.message)
            this.loadingStatus.loadingItem = ''
            this.$refs.userProductModal.hideModal()
            this.getCartInfo()
          } else {
            alert(res.data.message)
          }
        })
    }
  },
  mounted () {
    this.getData()
  }
}
</script>
