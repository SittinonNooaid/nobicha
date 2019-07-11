<template>
  <div class="container">
    <div>
      <h4>เพิ่มข้อมูลสินค้า</h4>
      <br />
      <b-form @submit="onSubmit" @reset="onReset">
        <b-form-input v-model="name_product" placeholder="ชื่อสินค้า"></b-form-input>
        <br />
        <b-form-input v-model="price_product" type="number" placeholder="ราคา"></b-form-input>
        <h5>สรุปข้อมุลเพิ่มสินค้า</h5>
        <div class="mt-2">: {{ name_product }}</div>
        <div class="mt-2">: {{ price_product }}</div>
        <b-button type="submit" variant="primary">เพิ่มสินค้า</b-button>
        <b-button type="reset" variant="danger">ลบข้อมูล</b-button>
      </b-form>
    </div>

    <div>
      <h4>แสดงรายการสินค้า</h4>
      <b-row class="text-center">
        <div v-for="item in products" v-bind:key="item.name">
          <div>
            <b-card
              v-bind:title="item.name"
              img-src="https://www.smeleader.com/wp-content/uploads/2019/01/%E0%B9%82%E0%B8%99%E0%B8%9A%E0%B8%B4%E0%B8%8A%E0%B8%B2-%E0%B9%81%E0%B8%9F%E0%B8%A3%E0%B8%99%E0%B9%84%E0%B8%8A%E0%B8%AA%E0%B9%8C%E0%B8%8A%E0%B8%B2%E0%B8%99%E0%B8%A1%E0%B9%84%E0%B8%82%E0%B9%88%E0%B8%A1%E0%B8%B8%E0%B8%81-%E0%B8%AB%E0%B8%A5%E0%B8%B1%E0%B8%81%E0%B9%81%E0%B8%AA%E0%B8%99%E0%B8%95%E0%B9%89%E0%B8%99%E0%B9%86-%E0%B8%9E%E0%B8%A3%E0%B9%89%E0%B8%AD%E0%B8%A1%E0%B9%80%E0%B8%9B%E0%B8%B4%E0%B8%94%E0%B8%82%E0%B8%B2%E0%B8%A2%E0%B8%9A%E0%B8%99%E0%B8%AB%E0%B9%89%E0%B8%B2%E0%B8%87-5.jpg"
              img-alt="Image"
              img-top
              tag="article"
              style="max-width: 15rem;"
              class="mb-2"
            >
              <b-card-text>{{ item.price }} บาท</b-card-text>

              <b-button v-on:click="addorder(item)" variant="primary">เลือก</b-button>
            </b-card>
          </div>
        </div>
      </b-row>
    </div>

    <div class>
      <h4>สั่งซื้อสินค้า</h4>
      <ul id="example-1">
        <li v-for="order in orders" v-bind:key="order.name">
          {{ order.name }}
          <b-form-input v-model="order.number" type="number" min="1"></b-form-input>
          <b-form-checkbox
            id="checkbox-1"
            v-model="order.status"
            name="checkbox-1"
            value="A"
            unchecked-value="B"
            @change="order_list_price($event, order)"
          >เพิ่มมุก</b-form-checkbox>

          <div>
            State:
            <strong>{{ order.status }}</strong>
          </div>
          {{ order.price }}
          // //
        </li>
      </ul>
      <h4>ราคาสุทธิ:{{order_total_price}}</h4>
    </div>
  </div>
</template>

<script>
import Logo from "~/components/Logo.vue";

export default {
  components: {
    Logo
  },
  data() {
    return {
      name_product: "",
      price_product: 0,
      products: [
        {
          name: "ชานมใต้หวัน1",
          price: 19
        },
        {
          name: "ชานมใต้หวัน2",
          price: 19
        },
        {
          name: "ชานมใต้หวัน3",
          price: 19
        }
      ],
      orders: []
    };
  },
  methods: {
    addorder(item) {
      this.orders.push({
        name: item.name,
        price: item.price,
        number: 1,
        status: "B"
      });
    },
    addProduct(evt) {
      this.products.push({
        name: this.name_product,
        price: this.price_product
      });
      this.onReset(evt);
    },
    onSubmit(evt) {
      evt.preventDefault();
      if (this.name_product == "") {
        alert("กรุณาเพิ่มชื่อสินค้า");
        return false;
      }
      if (this.price_product == "" || this.price_product <= 0) {
        alert("กรุณา.....");
        return false;
      }
      this.addProduct(evt);
    },
    onReset(evt) {
      evt.preventDefault();
      // Reset our form values
      (this.name_product = ""), (this.price_product = 0);
    },
    order_list_price: function(event, order) {
      if (event === "A") {
        order.price += 5;
      } else {
        order.price -= 5;
      }
    }
  },
  computed: {
    // a computed getter

    order_total_price: function() {
      const total = this.orders.reduce((sum, order) => {
        return (sum += order.price);
      }, 0);
      return total;
    }
  }
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
