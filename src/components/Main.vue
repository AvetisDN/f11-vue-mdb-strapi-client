<template>
  <Header />
  <MDBContainer class="mt-3 py-3">
    <MDBRow>
      <MDBCol class="d-flex justify-content-end pb-3 align-items-center">
        View
        <select class="form-control w-auto mx-3" v-model="perPage" @change="setPage(1)">
          <option :value="4">4</option>
          <option :value="8">8</option>
          <option :value="12">12</option>
          <option :value="products.length">All</option>
        </select>
        per page
      </MDBCol>
    </MDBRow>
    <MDBRow>
      <MDBCol
        col="12"
        md="6"
        lg="4"
        xl="3"
        class="mb-4"
        v-for="product in visibleProducts"
        :key="product.id"
      >
        <MDBCard>
          <MDBCardImg :src="baseURL + product.image.url" :alt="product.title" />
          <MDBCardBody>
            <MDBCardTitle>
              {{ product.title }}
            </MDBCardTitle>
            <MDBCardText>
              {{ product.description.substr(0, 20) + "..." }}
            </MDBCardText>
          </MDBCardBody>
        </MDBCard>
      </MDBCol>
    </MDBRow>
    <nav aria-label="Page navigation example" v-if="lastPage > 1">
      <MDBPagination class="justify-content-center">
        <MDBPageNav prev :disabled="page === 1" @click.prevent="setPage(page - 1)" />
        <MDBPageItem
          v-for="num in lastPage"
          :active="num === page"
          :key="num"
          href="#"
          @click.prevent="setPage(num)"
        >
          {{ num }}
        </MDBPageItem>
        <MDBPageNav
          next
          :disabled="page === lastPage"
          @click.prevent="setPage(page + 1)"
        />
      </MDBPagination>
    </nav>
  </MDBContainer>
</template>

<script>
import { ref } from "@vue/reactivity";
import {
  MDBContainer,
  MDBRow,
  MDBCol,
  MDBCard,
  MDBCardTitle,
  MDBCardBody,
  MDBCardText,
  MDBCardImg,
  MDBPagination,
  MDBPageNav,
  MDBPageItem,
} from "mdb-vue-ui-kit";
import Header from "./Header.vue";
export default {
  name: "Main",
  components: {
    MDBContainer,
    MDBRow,
    MDBCol,
    MDBCard,
    MDBCardTitle,
    MDBCardBody,
    MDBCardText,
    MDBCardImg,
    MDBPagination,
    MDBPageNav,
    MDBPageItem,
    Header,
  },
  setup() {
    const products = ref([]);
    const visibleProducts = ref([]);
    const baseURL = ref("http://localhost:1337");
    const perPage = ref(4);
    const page = ref(1);
    const total = ref(0);
    const lastPage = ref(1);

    const getVisibleProducts = () => {
      visibleProducts.value = products.value.slice(
        (page.value - 1) * perPage.value,
        page.value * perPage.value
      );
      lastPage.value = Math.ceil(products.value.length / perPage.value);
    };

    const setPage = (num) => {
      if(num < 1 || num > lastPage.value) return null
      page.value = num;
      getVisibleProducts();
    };

    fetch(`${baseURL.value}/products`)
      .then((res) => res.json())
      .then((data) => {
        products.value = data;
        total.value = data.length;
        lastPage.value = Math.ceil(products.value.length / perPage.value);
        getVisibleProducts();
      });

    return {
      products,
      baseURL,
      perPage,
      page,
      total,
      lastPage,
      visibleProducts,
      setPage,
    };
  },
};
</script>

<style>
.card-img {
  background: var(--mdb-dark);
}
</style>