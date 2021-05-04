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
          </tbody>
          <tfoot>
            <tr>
              <td colspan="2"></td>
              <td>總價</td>
              <td><span class="total-price">$0</span></td>
              <td></td>
            </tr>
          </tfoot>
        </table>
        <button class="btn btn-lg btn-success empty-cart"><i class="fas fa-baby-carriage"></i> 清空購物車</button>
      </section>
    </section>
  </main>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const itemList = ref([]);
    
    fetch('./item.json')
      .then(res => res.json())
      .then(d => itemList.value = itemList.value.concat(d));

    return {
      itemList
    }
  }
}
</script>

<style>

</style>
