<template>
  <!-- Template untuk menampilkan produk dan pesan -->
  <div :class="getClassForCategory()" class="container">
    <div class="product-card">
      <!-- Tampilkan pesan loading saat data sedang diambil -->
      <div v-if="loading" class="loading">
        <div class="spinner">
          <i class="fa fa-spinner fa-spin"></i>
        </div>
      </div>

      <!-- Tampilkan detail produk jika data tersedia -->
      <div class="product-details" v-else-if="product">
        <div class="product-image">
          <img :src="product.image" alt="image" width="200px" />
        </div>
        <div class="product-detail">
          <h2 class="title">{{ product.title }}</h2>
          <div class="rating">
            <div>{{ product.category }}</div>
            <div class="rate">
              {{ product.rating.rate }}/5
              <!-- Menampilkan bintang rating -->
              <span class="full-circle" style="margin-left: 3px"> </span>
              <span class="full-circle"> </span>
              <span class="full-circle"> </span>
              <span class="circle"> </span>
              <span class="circle"> </span>
            </div>
          </div>
          <hr />
          <p class="description" style="min-height: 280px">{{ product.description }}</p>
          <hr />
          <h3 class="price">${{ product.price }}</h3>
          <div class="button">
            <button class="button-buy">Buy Now</button>
            <button class="button-next" @click="getNextProduct">Next Product</button>
          </div>
        </div>
      </div>

      <!-- Tampilkan pesan jika produk tidak tersedia -->
      <div v-else>
        <p>This product is unavailable to show</p>
        <button class="button-next" @click="getNextProduct">Next Product</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      index: 1, // Nomor produk saat ini
      product: null, // Detail produk
      loading: false, // Indikator loading
    };
  },
  methods: {
    // Mengambil produk berikutnya dari API
    getNextProduct() {
      this.index = this.index === 20 ? 1 : this.index + 1;
      this.loading = true; // Aktifkan loading

      // Delay simulasi pengambilan data
      setTimeout(() => {
        fetch(`https://fakestoreapi.com/products/${this.index}`)
          .then((response) => response.json())
          .then((data) => {
            if (data.category === "men's clothing" || data.category === "women's clothing") {
              this.product = data;
            } else {
              this.product = null; // Produk tidak valid, tampilkan pesan
            }
          })
          .catch((error) => {
            console.error(error);
          })
          .finally(() => {
            this.loading = false; // Matikan loading setelah data diambil
          });
      }, 500); // Menunggu 0.5 detik sebelum mengambil data (simulasi)
    },

    // Menentukan kelas CSS berdasarkan kategori produk
    getClassForCategory() {
      if (this.product) {
        if (this.product.category === "men's clothing") {
          return "page-men";
        } else if (this.product.category === "women's clothing") {
          return "page-women";
        }
      }
      return "page-unavailable";
    },
  },
  mounted() {
    this.getNextProduct(); // Memuat produk pertama saat komponen dimuat
  },
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-content: center;
}

/* styles.css */
/* Desain Men's Section */
.page-men {
  content: ""; /* Elemen pseudo untuk setengah latar belakang */
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 60%; /* Setengah latar belakang */
  background-color: #d6e6ff; /* Warna setengah latar belakang */
  z-index: -1;
  background-image: url("../assets/bg-pattern.svg");
}

/* Desain Women's Section */
.page-women {
  content: ""; /* Elemen pseudo untuk setengah latar belakang */
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 60%; /* Setengah latar belakang */
  background-color: #fde2ff; /* Warna setengah latar belakang */
  z-index: -1;
  background-image: url("../assets/bg-pattern.svg");
}

.rate {
  display: flex;
  gap: 2px;
}
.page-men .full-circle {
  display: flex;
  width: 18px;
  height: 18px;
  background: #002772;
  border-radius: 100%;
}
.page-men .circle {
  display: flex;
  width: 13.5px;
  height: 13.5px;
  border: solid #002772;
  border-radius: 100%;
}

.page-women .full-circle {
  display: flex;
  width: 18px;
  height: 18px;
  background: #720060;
  border-radius: 100%;
}
.page-women .circle {
  display: flex;
  width: 13.5px;
  height: 13.5px;
  border: solid #720060;
  border-radius: 100%;
}

/* Desain Unavailable Product */
.page-unavailable {
  content: ""; /* Elemen pseudo untuk setengah latar belakang */
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 60%; /* Setengah latar belakang */
  background-color: #dcdcdc; /* Warna setengah latar belakang */
  z-index: -1;
}

/* Styling container card produk */
.product-card {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-content: center;
  align-items: center;
  background-color: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 20px;
  border-radius: 10px;
  margin: 20px;
  height: 580px;
  width: 1034px;
  margin-top: 50px;
  box-shadow: 0px 4px 4px 0px #00000040;
}

.product-details {
  display: flex;
}

.product-detail {
  position: relative;
  top: -40px;
  text-align: left;
  margin-left: 50px;
  width: 548px;
  height: 500px;
}

.product-image {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 20px;
}

img {
  width: 306px;
  height: 407px;
}

.title {
  font-size: 30px;
  font-weight: 600;
  line-height: 33.89px;
  margin-bottom: 10px;
}

.price {
  font-size: 28px;
  font-weight: 600;
  line-height: 33.89px;
  margin-top: 5px;
  margin-bottom: 3px;
}

.page-men .title,
.page-men .price {
  color: #002772;
}

.page-women .title,
.page-women .price {
  color: #720060;
}

.rating {
  display: flex;
  justify-content: space-between;
}

button {
  cursor: pointer;
}

.button {
  margin-top: 10px;
  display: flex;
  justify-content: space-between;
}
.page-men .button-buy {
  width: 259px;
  height: 42px;
  border-radius: 4px;
  text-align: center;
  background-color: #002772;
  color: #ffffff;
  font-family: Inter;
  font-size: 20px;
  font-weight: 600;
  line-height: 24px;
  letter-spacing: 0em;
}

.page-men .button-next {
  width: 259px;
  height: 42px;
  border-radius: 4px;
  text-align: center;
  border: 3px solid #002772;
  color: #002772;
  background-color: #ffffff;
  font-family: Inter;
  font-size: 20px;
  font-weight: 600;
  line-height: 24px;
  letter-spacing: 0em;
}

.page-women .button-buy {
  width: 259px;
  height: 42px;
  border-radius: 4px;
  text-align: center;
  background-color: #720060;
  color: #ffffff;
  font-family: Inter;
  font-size: 20px;
  font-weight: 600;
  line-height: 24px;
  letter-spacing: 0em;
}

.page-women .button-next {
  width: 259px;
  height: 42px;
  border-radius: 4px;
  text-align: center;
  border: 3px solid #720060;
  color: #720060;
  background-color: #ffffff;
  font-family: Inter;
  font-size: 20px;
  font-weight: 600;
  line-height: 24px;
  letter-spacing: 0em;
}

.page-unavailable .product-card {
  background-image: url(../assets//sad-face.png);
  background-size: cover; /* Menyesuaikan gambar untuk mengisi elemen */
  background-repeat: no-repeat; /* Mencegah pengulangan gambar */
  background-position: left; /* Tengahkan gambar dalam elemen */
  background-size: contain;
}

.description {
  font-family: Inter;
  font-size: 18px;
  font-weight: 400;
  line-height: 24px;
  letter-spacing: 0em;
  color: #1e1e1e;
}

.page-unavailable .button-next {
  width: 465px;
  height: 42px;
  top: 409px;
  left: 431px;
  border-radius: 4px;
  border: 3px solid #000000;
  font-size: 20px;
  font-weight: 600;
  line-height: 24px;
  letter-spacing: 0em;
  background-color: #ffffff;
}

/* Gaya spinner */
.spinner {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 40px; /* Sesuaikan dengan tinggi yang sesuai */
}

.spinner i {
  font-size: 80px; /* Sesuaikan ukuran ikon spinner */
  color: #333; /* Sesuaikan warna ikon spinner */
}
</style>
