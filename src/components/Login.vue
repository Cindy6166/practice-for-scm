<script setup lang="ts">
import { reactive, ref } from "vue";
import router from "../router";
import { useAuthStore } from "../store/authStore";
import Modal from "./Modal.vue";
import Spinner from "./Spinner.vue";

const userInfo = reactive({ username: "", password: "" });
const errorModal = reactive({
  msg: "",
  showModal: false,
});
const isLoading = ref(false);
const onLogin = async (userInfo: Object) => {
  isLoading.value = true;
  try {
    await useAuthStore().Login(userInfo);
  } catch (err: any) {
    errorModal.msg = err;
    errorModal.showModal = true;
  }
  router.push("/");
};
const onClose = () => {
  errorModal.showModal = false;
  isLoading.value = false;
  userInfo.username = "";
  userInfo.password = "";
};
</script>

<template>
  <Modal :set-modal="errorModal" @close="onClose" />
  <section class="absolute w-full h-full">
    <div
      class="absolute top-0 w-full h-full bg-gray-900 bg-cover bg-no-repeat bg-[url('src/assets/img/register_bg_2.png')]"
    ></div>
    <div class="container mx-auto px-4 h-full">
      <div class="flex content-center items-center justify-center h-full">
        <div class="w-full lg:w-4/12 px-4">
          <div
            class="relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded-lg bg-gray-300 border-0"
          >
            <div class="rounded-t mb-0 px-6 py-6">
              <div class="text-center mb-3">
                <h6 class="text-gray-600 text-sm font-bold">Login</h6>
              </div>
              <hr class="mt-6 border-b-1 border-gray-400" />
            </div>
            <div class="flex-auto px-4 lg:px-10 py-10 pt-0">
              <div class="text-gray-500 text-center mb-3 font-bold"></div>
              <form>
                <div class="relative w-full mb-3">
                  <label
                    class="block uppercase text-gray-700 text-xs font-bold mb-2"
                    for="grid-password"
                    >USERNAME</label
                  ><input
                    v-model="userInfo.username"
                    type="text"
                    class="border-0 px-3 py-3 placeholder-gray-400 text-gray-700 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full"
                    placeholder="Username"
                  />
                </div>
                <div class="relative w-full mb-3">
                  <label
                    class="block uppercase text-gray-700 text-xs font-bold mb-2"
                    for="grid-password"
                    >Password</label
                  ><input
                    v-model="userInfo.password"
                    type="password"
                    class="border-0 px-3 py-3 placeholder-gray-400 text-gray-700 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full"
                    placeholder="Password"
                    @keyup.enter="onLogin(userInfo)"
                  />
                </div>
                <div class="text-center mt-6">
                  <button
                    class="transition ease-in-out delay-150 hover:-translate-y-1 hover:scale-110 hover:bg-cyan-500 duration-300 flex justify-center items-center bg-gray-900 text-white active:bg-gray-700 text-sm font-bold uppercase px-6 py-3 rounded shadow hover:shadow-lg outline-none focus:outline-none mr-1 mb-1 w-full"
                    type="button"
                    @click.prevent="onLogin(userInfo)"
                  >
                    <Spinner :is-loading="isLoading" />
                    <span>Login</span>
                  </button>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
