<template>
    <div>
        <h1 class="pt-3">{{ product.title }}</h1>
        <div class="row">
        <div class="col-4 pt-3 pb-5">
            <img :src="product.imageUrl" alt="" class="img-fluid">
        </div>
        <div class="col-6 pt-3 pb-5">
            <table class="table">
  <thead>
    <tr>
      <th scope="col"></th>
      <th scope="col">分類</th>
      <th scope="col">商品描述</th>
      <th scope="col">售價</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1</th>
      <td>{{ product.category }}</td>
      <td>{{ product.description }}</td>
      <td>{{ product.price }}</td>
    </tr>
  </tbody>
</table>
        </div>
        </div>
    </div>
</template>

<script>
export default {
  data () {
    return {
      product: {}
    }
  },
  mounted () {
    const { id } = this.$route.params
    const url = `${process.env.VUE_APP_API}/${process.env.VUE_APP_PATH}/product/${id}`
    this.$http.get(url).then((res) => {
      if (res.data.success) {
        this.product = res.data.product
      } else {
        alert(res.data.message)
      }
    })
  }
}
</script>
