<template>
  <div class="p-3 flex flex-col bg-main-color h-screen fixed overflow-hidden transition-all duration-300 shadow-lg z-9"
    :class="{ 'w-[250px]': !collapsed, 'w-0 invisible': collapsed }">
    <!-- Logo Area -->
    <div class="flex-grow">
      <div
        class="flex items-center justify-center text-center mb-auto p-2 hover:scale-105 transition-transform duration-300">
        <Link :href="route('dashboard')" class="inline-block relative group cursor-pointer">
        <img :src="logoPath" alt="Logo" class="object-cover relative z-10" />
        </Link>
      </div>
      <hr class="mx-3 mb-5 mt-5 border-t-[3px] border-white opacity-70 rounded" />

      <!-- Secciones -->
      <div
        class="max-h-[calc(100vh-250px)] overflow-auto scrollbar-thin scrollbar-thumb-gray-300 scrollbar-track-gray-100">
        <ul class="list-unstyled flex flex-col gap-1.5">
          <li>
            <button
              class="w-full text-left text-white grid grid-cols-[24px_1fr_24px] items-center gap-3 px-4 py-3 hover:bg-white/10 rounded-lg transition-all duration-200 group cursor-pointer"
              @click="toggleSubmenu('homeSubmenu')">
              <i class="fa-solid fa-house text-center w-5 text-white group-hover:text-white"></i>
              <span class="whitespace-nowrap group-hover:translate-x-1 transition-transform duration-200">Home</span>
              <svg class="ml-auto w-4 h-4 transition-transform duration-300"
                :class="{ 'rotate-180': openSubmenus.homeSubmenu }" fill="none" stroke="currentColor"
                viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
              </svg>
            </button>

            <ul class="relative overflow-hidden transition-all duration-300"
              :style="{ maxHeight: openSubmenus.homeSubmenu ? homeSubmenuHeight + 'px' : '0px' }" ref="homeSubmenu">
              <div class="absolute left-6 w-0.5 h-25 bg-gray-200 opacity-80 rounded-sm mr-3"></div>
              <li>
                <Link :href="route('slider.dashboard')"
                  class="block pl-10 py-2 text-gray-200 hover:text-white rounded-lg transition-all duration-200 my-1 cursor-pointer">
                <span class="opacity-80 hover:opacity-100 transition-opacity flex items-center">
                  <i class="fa-solid fa-image-portrait w-5 mr-2 text-xs"></i>
                  Slider
                </span>
                </Link>
              </li>
              <li>
                <Link :href="route('contenido.dashboard')"
                  class="block pl-10 py-2 text-gray-200 hover:text-white rounded-lg transition-all duration-200 my-1 cursor-pointer">
                <span class="opacity-80 hover:opacity-100 transition-opacity flex items-center">
                  <i class="fa-solid fa-align-left w-5 mr-2 text-xs"></i>
                  Contenido
                </span>
                </Link>
              </li>
            </ul>
          </li>
          <li>
            <button
              class="w-full text-left text-white grid grid-cols-[24px_1fr_24px] items-center gap-3 px-4 py-3 hover:bg-white/10 rounded-lg transition-all duration-200 group cursor-pointer"
              @click="toggleSubmenu('nosotrosSubmenu')">
              <i class="fa-solid fa-address-card text-center w-5 text-white"></i>
              <span
                class="whitespace-nowrap group-hover:translate-x-1 transition-transform duration-200">Nosotros</span>
              <svg class="ml-auto w-4 h-4 transition-transform duration-300"
                :class="{ 'rotate-180': openSubmenus.nosotrosSubmenu }" fill="none" stroke="currentColor"
                viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
              </svg>
            </button>

            <ul class="relative overflow-hidden transition-all duration-300"
              :style="{ maxHeight: openSubmenus.nosotrosSubmenu ? nosotrosSubmenuHeight + 'px' : '0px' }"
              ref="nosotrosSubmenu">
              <div class="absolute left-6 w-0.5 h-25 bg-gray-200 opacity-80 rounded-sm mr-3"></div>
              <li>
                <Link :href="route('banners.dashboard')"
                  class="block pl-10 py-2 text-gray-200 hover:text-white rounded-lg transition-all duration-200 my-1 cursor-pointer">
                <span class="opacity-80 hover:opacity-100 transition-opacity flex items-center">
                  <i class="fa-solid fa-panorama w-5 mr-2 text-xs"></i>
                  Banners
                </span>
                </Link>
              </li>
              <li>
                <Link :href="route('nosotros.dashboard')"
                  class="block pl-10 py-2 text-gray-200 hover:text-white rounded-lg transition-all duration-200 my-1 cursor-pointer">
                <span class="opacity-80 hover:opacity-100 transition-opacity flex items-center">
                  <i class="fa-solid fa-align-left w-5 mr-2 text-xs"></i>
                  Contenido
                </span>
                </Link>
              </li>
            </ul>
          </li>
          <li>
            <button
              class="w-full text-left text-white grid grid-cols-[24px_1fr_24px] items-center gap-3 px-4 py-3 hover:bg-white/10 rounded-lg transition-all duration-200 group cursor-pointer"
              @click="toggleSubmenu('productosSubmenu')">
              <i class="fa-solid fa-cart-shopping text-center w-5 text-white group-hover:text-white"></i>
              <span
                class="whitespace-nowrap group-hover:translate-x-1 transition-transform duration-200">Productos</span>
              <svg class="ml-auto w-4 h-4 transition-transform duration-300"
                :class="{ 'rotate-180': openSubmenus.productosSubmenu }" fill="none" stroke="currentColor"
                viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
              </svg>
            </button>

            <ul class="relative overflow-hidden transition-all duration-300"
              :style="{ maxHeight: openSubmenus.productosSubmenu ? productosSubmenuHeight + 'px' : '0px' }"
              ref="productosSubmenu">
              <div class="absolute left-6 w-0.5 h-20 bg-gray-200 opacity-80 rounded-sm mr-3"></div>
              <li>
                <Link :href="route('categorias.dashboard')"
                  class="block pl-10 py-2 text-gray-200 hover:text-white rounded-lg transition-all duration-200 my-1 cursor-pointer">
                <span class="opacity-80 hover:opacity-100 transition-opacity flex items-center">
                  <i class="fa-solid fa-layer-group w-5 mr-2 text-xs"></i>
                  Categorías
                </span>
                </Link>
              </li>
              <li>
                <Link :href="route('productos.dashboard')"
                  class="block pl-10 py-2 text-gray-200 hover:text-white rounded-lg transition-all duration-200 my-1 cursor-pointer">
                <span class="opacity-80 hover:opacity-100 transition-opacity flex items-center">
                  <i class="fa-solid fa-box-open w-5 mr-2 text-xs"></i>
                  Productos
                </span>
                </Link>
              </li>
            </ul>
          </li>
          <li>
            <Link :href="route('aplicaciones.dashboard')"
              class="grid grid-cols-[24px_1fr_24px] items-center gap-3 px-4 py-3 text-white hover:bg-white/10 rounded-lg transition-all duration-200 group cursor-pointer">
            <i class="fa-solid fa-puzzle-piece text-center w-5 text-white"></i>
            <span class="whitespace-nowrap group-hover:translate-x-1 transition-transform duration-200">Aplicaciones</span>
            <span></span>
            </Link>
          </li>
          <li>
            <Link :href="route('colores.dashboard')"
              class="grid grid-cols-[24px_1fr_24px] items-center gap-3 px-4 py-3 text-white hover:bg-white/10 rounded-lg transition-all duration-200 group cursor-pointer">
            <i class="fa-solid fa-palette text-center w-5 text-white"></i>
            <span class="whitespace-nowrap group-hover:translate-x-1 transition-transform duration-200">Carta de
              colores</span>
            <span></span>
            </Link>
          </li>
          <li>
            <Link :href="route('calidad.dashboard')"
              class="grid grid-cols-[24px_1fr_24px] items-center gap-3 px-4 py-3 text-white hover:bg-white/10 rounded-lg transition-all duration-200 group cursor-pointer">
            <i class="fa-solid fa-star text-center w-5 text-white"></i>
            <span class="whitespace-nowrap group-hover:translate-x-1 transition-transform duration-200">Calidad</span>
            <span></span>
            </Link>
          </li>
          <li>
            <Link :href="route('novedades.dashboard')"
              class="grid grid-cols-[24px_1fr_24px] items-center gap-3 px-4 py-3 text-white hover:bg-white/10 rounded-lg transition-all duration-200 group cursor-pointer">
            <i class="fa-solid fa-newspaper text-center w-5 text-white"></i>
            <span class="whitespace-nowrap group-hover:translate-x-1 transition-transform duration-200">Novedades</span>
            <span></span>
            </Link>
          </li>
          <li>
            <Link :href="route('contacto.dashboard')"
              class="grid grid-cols-[24px_1fr_24px] items-center gap-3 px-4 py-3 text-white hover:bg-white/10 rounded-lg transition-all duration-200 group cursor-pointer">
            <i class="fa-solid fa-square-phone text-center w-5 text-white"></i>
            <span class="whitespace-nowrap group-hover:translate-x-1 transition-transform duration-200">Contacto</span>
            <span></span>
            </Link>
          </li>
          <hr class="mx-6 my-2 border-t-[1px] border-white/30" />
          <li>
            <Link :href="route('logos.dashboard')"
              class="grid grid-cols-[24px_1fr_24px] items-center gap-3 px-4 py-3 text-white hover:bg-white/10 rounded-lg transition-all duration-200 group relative overflow-hidden cursor-pointer">
            <div
              class="absolute left-0 top-0 h-full w-0 bg-white/10 transition-all duration-300 group-hover:w-full rounded-lg">
            </div>
            <i class="fa-solid fa-image text-center w-5 text-white relative z-10"></i>
            <span
              class="whitespace-nowrap group-hover:translate-x-1 transition-transform duration-200 relative z-10">Logos</span>
            <span class="relative z-10"></span>
            </Link>
          </li>
          <li>
            <Link :href="route('usuarios.dashboard')"
              class="grid grid-cols-[24px_1fr_24px] items-center gap-3 px-4 py-3 text-white hover:bg-white/10 rounded-lg transition-all duration-200 group relative overflow-hidden cursor-pointer">
            <div
              class="absolute left-0 top-0 h-full w-0 bg-white/10 transition-all duration-300 group-hover:w-full rounded-lg">
            </div>
            <i class="fa-solid fa-users text-center w-5 text-white relative z-10"></i>
            <span
              class="whitespace-nowrap group-hover:translate-x-1 transition-transform duration-200 relative z-10">Usuarios</span>
            <span class="relative z-10"></span>
            </Link>
          </li>
          <li>
            <Link :href="route('newsletter.dashboard')"
              class="grid grid-cols-[24px_1fr_24px] items-center gap-3 px-4 py-3 text-white hover:bg-white/10 rounded-lg transition-all duration-200 group relative overflow-hidden cursor-pointer">
            <div
              class="absolute left-0 top-0 h-full w-0 bg-white/10 transition-all duration-300 group-hover:w-full rounded-lg">
            </div>
            <i class="fa-solid fa-inbox text-center w-5 text-white relative z-10"></i>
            <span
              class="whitespace-nowrap group-hover:translate-x-1 transition-transform duration-200 relative z-10">Newsletter</span>
            <span class="relative z-10"></span>
            </Link>
          </li>
          <li>
            <Link :href="route('metadatos.dashboard')"
              class="grid grid-cols-[24px_1fr_24px] items-center gap-3 px-4 py-3 text-white hover:bg-white/10 rounded-lg transition-all duration-200 group relative overflow-hidden cursor-pointer">
            <div
              class="absolute left-0 top-0 h-full w-0 bg-white/10 transition-all duration-300 group-hover:w-full rounded-lg">
            </div>
            <i class="fa-solid fa-magnifying-glass text-center w-5 text-white relative z-10"></i>
            <span
              class="whitespace-nowrap group-hover:translate-x-1 transition-transform duration-200 relative z-10">Metadatos</span>
            <span class="relative z-10"></span>
            </Link>
          </li>
        </ul>
      </div>
    </div>

    <!-- Footer del sidebar -->
    <div class="mt-auto">
      <hr class="mx-3 my-3 border-t-[3px] border-white opacity-70 rounded" />
      <p class="text-gray-300 text-xs text-center px-3">
        <span class="opacity-70 hover:opacity-100 transition-opacity duration-200">@2025 Nahmias - by Osole</span>
      </p>
    </div>
  </div>
</template>

<script>
import { Link } from '@inertiajs/vue3';
import { route } from 'ziggy-js';

export default {
  components: {
    Link
  },
  props: {
    logo: {
      type: Object,
      default: () => ({ path: 'images/default-logo.png' })
    },
    collapsed: {
      type: Boolean,
      default: false,
    }
  },
  data() {
    return {
      logoPath: '',
      openSubmenus: {
        homeSubmenu: false,
        nosotrosSubmenu: false,
        productosSubmenu: false,
      },
      homeSubmenuHeight: 0,
      nosotrosSubmenuHeight: 0,
      productosSubmenuHeight: 0,
    };
  },
  methods: {
    async toggleSubmenu(submenuId) {
      this.openSubmenus[submenuId] = !this.openSubmenus[submenuId];

      // Wait for DOM update to calculate heights
      await this.$nextTick();

      // Use $refs directly to access the DOM elements
      if (submenuId === 'homeSubmenu' && this.$refs.homeSubmenu) {
        this.homeSubmenuHeight = this.$refs.homeSubmenu.scrollHeight;
      }
      if (submenuId === 'nosotrosSubmenu' && this.$refs.nosotrosSubmenu) {
        this.nosotrosSubmenuHeight = this.$refs.nosotrosSubmenu.scrollHeight;
      }
      if (submenuId === 'productosSubmenu' && this.$refs.productosSubmenu) {
        this.productosSubmenuHeight = this.$refs.productosSubmenu.scrollHeight;
      }
    }
  },
  mounted() {
    this.logoPath = this.logo && this.logo.path ? this.logo.path : 'images/default-logo.png';

    // Calculate submenu heights on mount
    this.$nextTick(() => {
      if (this.$refs.homeSubmenu) {
        this.homeSubmenuHeight = this.$refs.homeSubmenu.scrollHeight;
      }
      if (this.$refs.productosSubmenu) {
        this.productosSubmenuHeight = this.$refs.productosSubmenu.scrollHeight;
      }
      if (this.$refs.zonaSubmenu) {
        this.zonaSubmenuHeight = this.$refs.zonaSubmenu.scrollHeight;
      }
    });

    // Apply entry animations
    const sidebar = document.querySelector('.bg-main-color');
    const menuItems = document.querySelectorAll('a, button');

    if (sidebar) {
      sidebar.classList.add('animate-sidebar-in');

      // Animate each menu item with delay
      menuItems.forEach((item, index) => {
        setTimeout(() => {
          item.classList.add('animate-fade-in');
        }, 100 + (index * 50));
      });
    }
  }
}
</script>

<style scoped>
@keyframes sidebarIn {
  from {
    transform: translateX(-20px);
    opacity: 0;
  }

  to {
    transform: translateX(0);
    opacity: 1;
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-sidebar-in {
  animation: sidebarIn 0.4s ease-out forwards;
}

.animate-fade-in {
  opacity: 0;
  animation: fadeIn 0.3s ease-out forwards;
}

/* Personaliza la barra de desplazamiento */
.overflow-auto::-webkit-scrollbar {
  width: 6px;
}

.overflow-auto::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
}

.overflow-auto::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.3);
  border-radius: 10px;
}

.overflow-auto::-webkit-scrollbar-thumb:hover {
  background: rgba(255, 255, 255, 0.5);
}
</style>