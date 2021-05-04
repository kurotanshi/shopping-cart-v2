<template>
  <main>
    <header>
      <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <div class="container-fluid">
          <a class="navbar-brand" href="/"><i class="fas fa-gem"></i> 賺很大商店</a>
          <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
              <li class="nav-item">
                <a class="nav-link active" aria-current="page" href="#">商品</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="#">當日特價</a>
              </li>
            </ul>
          </div>
        </div>
      </nav>
    </header>

    <section class="container mt-4">
    <div class="items row">
      <div
        class="col-sm-2"
        v-for="item in itemList"
        :key="item.id"
      >
        <div
          :data-product-id="item.id"
          class="card"
        >
          <img
            :src="`./images/${item.cover}`"
            :alt="item.name"
            class="card-img-top"
          >
          <div class="card-body">
            <h5 class="card-title fs-6 fw-light">{{ item.name }}</h5>
            <p class="price">${{ item.price }}</p>
            <button
              @click="addToCart(item)"
              class="btn btn-sm btn-warning fw-light"
            ><i class="fas fa-cat"></i></button>
          </div>
        </div>
      </div>
    </div>
      <hr>

      <section class="cart">
        <h2>購物車</h2>
        <table class="table cart-item-table">
          <thead>
            <tr>
              <th scope="col">項目</th>
              <th scope="col">數量</th>
              <th scope="col">單價</th>
              <th scope="col">小計</th>
              <th scope="col"></th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="c in cartList"
              :key="c.id"
            >
              <td>{{ c.name }}</td>
              <td>
                <input
                  type="number"
                  min="1"
                  class="quantity"
                  :value="c.qty">
                </td>
              <td>${{ c.price }}</td>
              <td>${{ c.price * c.qty }}</td>
              <td><button
                  @click="removeFromCart(c.id)"
                  :data-id="c.id"
                  class="remove-item-btn btn btn-danger btn-sm"
                >
                  <i class="fas fa-trash-alt"></i></button>
              </td>
            </tr>
          </tbody>
          <tfoot>
            <tr>
              <td colspan="2"></td>
              <td>總價</td>
              <td><span class="total-price">${{ totalPrice }}</span></td>
              <td></td>
            </tr>
          </tfoot>
        </table>
        <button 
          @click="cleanCart"
          class="btn btn-lg btn-success empty-cart"><i class="fas fa-baby-carriage"></i> 清空購物車</button>
      </section>
    </section>
  </main>
</template>

<script>
import { ref, computed } from 'vue';

export default {
  setup() {
    
    // 商品列表
    const itemList = ref([]);
    
    fetch('./item.json')
      .then(res => res.json())
      .then(d => itemList.value = itemList.value.concat(d));


    // 購物車列表
    const cartList = ref([]);
    
    // 加入購物車
    const addToCart = (item) => {
      const idx = cartList.value.findIndex((d) => item.id === d.id);
      if (idx > -1) {
        cartList.value[idx].qty++;
      } else {
        cartList.value.push({
          ...item,
          qty: 1,
        });
      }
    };

    // 從購物車移除
    const removeFromCart = (itemId) => {
      cartList.value = cartList.value.filter((item) => item.id != itemId);
    };

    // 清除購物車
    const cleanCart = () => {
      cartList.value.length = 0
    };

    // 計算總價
    const totalPrice = computed(() => Math.round(cartList.value.reduce((sum, item) => sum + item.price * item.qty, 0) * 100) / 100);

    return {
      itemList,
      cartList,
      addToCart,
      removeFromCart,
      cleanCart,
      totalPrice
    }
  }
}
</script>

<style>

</style>
