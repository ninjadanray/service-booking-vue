<template>
    <nav class="flex flex-wrap items-center justify-between py-3 text-gray-700 bg-white container mx-auto px-4">
    <div class="block lg:hidden">
        <button class="flex items-center p-2 -mr-1 rounded-lg focus:bg-gray-100">
        <svg class="stroke-current text-gray-500 w-7 h-7" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><title>Menu</title><g fill="none"><path d="M4 6h16M4 12h8m-8 6h16" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path></g></svg>
        </button>
    </div>
    <div class="items-center w-full text-center lg:flex-1 lg:flex lg:text-left pt-2 lg:pt-0 hidden" data-nav-target="menu">
        <div class="lg:flex-grow">
        <span class="p-3 hover:bg-gray-100 transition ease duration-300 rounded-lg text-center focus:bg-gray-100 lg:inline-block block">
            <NavbarLink to="/" label="Service" />
        </span>
        <span v-if="isLogin" class="p-3 hover:bg-gray-100 transition ease duration-300 rounded-lg text-center focus:bg-gray-100 lg:inline-block block">
            <NavbarLink to="/cart" label="Cart" />
        </span>
        </div>
        <div v-if="!isLogin" class="items-center block w-full mt-2 text-center lg:flex lg:flex-row lg:flex-1 lg:mt-0 lg:text-left lg:justify-end">
        <span class="p-3 hover:bg-gray-100 transition ease duration-300 rounded-lg text-center focus:bg-gray-100 lg:inline-block block">
            <NavbarLink to="/login" label="Login" />
        </span>

        <span class="p-3 hover:bg-gray-100 transition ease duration-300 rounded-lg text-center focus:bg-gray-100 lg:inline-block block">
            <NavbarLink to="/register" label="Register" />
        </span>


            <span class="h-5 w-px bg-gray-200 mx-3"></span>

            <a href="https://github.com/ninjadanray/service-booking-vue" target="_blank" class="group lg:ml-2 flex justify-center">
            <svg class="fill-current text-gray-400 group-hover:text-gray-600" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/></svg>
            </a>
        </div>
        <button v-if="isLogin" @click="logout" class="p-3 hover:bg-gray-100 transition ease duration-300 rounded-lg text-center focus:bg-gray-100 lg:inline-block block"> Logout</button>

        <span class="h-5 w-px bg-gray-200 mx-3"></span>

         <span v-if="isLogin" class="p-3 hover:bg-gray-100 transition ease duration-300 rounded-lg text-center focus:bg-gray-100 lg:inline-block block">
            {{ cartCount }} items in the cart
        </span>
    </div>
    </nav>
</template>

<script setup>
import { useRouter } from 'vue-router';
import NavbarLink from "./NavbarLink";
import { ref, computed, onMounted, watch } from 'vue'

const route = useRouter();

const isAuthenticated = ref(!!localStorage.getItem('token'));

const isLogin = ref(isAuthenticated.value);

const logout = () => {
    localStorage.removeItem('token');

    if(!isAuthenticated.value) {
        route.push({ name: 'home' })
    } else {
        route.push({ name: 'login' })
    }
}

const cartItems = computed(() => {
  const items = localStorage.getItem('cartItems');
  return items ? JSON.parse(items) : [];
});

const cartCount = computed(() => {
  return cartItems.value.length;
});

watch(isAuthenticated, (newVal) => {
  isLogin.value = newVal;
});

</script>