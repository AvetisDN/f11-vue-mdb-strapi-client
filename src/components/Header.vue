<template>
  <header class="bg-dark shadow-sm">
    <MDBContainer>
      <MDBNavbar expand="lg" dark class="shadow-0" container>
        <MDBNavbarBrand href="#">
          <i class="fas fa-kiwi-bird"></i>
          <span class="px-2 d-block">kiwi</span>
        </MDBNavbarBrand>
        <MDBNavbarToggler
          target="#navbar"
          @click="collapse = !collapse"
        ></MDBNavbarToggler>
        <MDBCollapse v-model="collapse">
          <MDBNavbarNav
            collapse="navbar"
            class="flex-grow-1 justify-content-end"
          >
            <MDBNavbarItem to="/" active> Home </MDBNavbarItem>
            <MDBNavbarItem to="/"> About </MDBNavbarItem>
          </MDBNavbarNav>
        </MDBCollapse>
      </MDBNavbar>
    </MDBContainer>
  </header>
</template>

<script>
import { ref } from "@vue/reactivity";
import {
  MDBNavbar,
  MDBNavbarToggler,
  MDBNavbarNav,
  MDBNavbarItem,
  MDBCollapse,
  MDBNavbarBrand,
  MDBContainer,
} from "mdb-vue-ui-kit";
export default {
  name: "Header",
  components: {
    MDBNavbar,
    MDBNavbarToggler,
    MDBNavbarNav,
    MDBNavbarItem,
    MDBCollapse,
    MDBNavbarBrand,
    MDBContainer,
  },
  setup() {
    const collapse = ref(false);
    return { collapse };
  },
}
</script>

<style scoped>

</style>