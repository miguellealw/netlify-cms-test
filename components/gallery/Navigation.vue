<template>
  <nav>
    <!-- Nav Icon -->
    <span @click="handleOpenNav">
      <svg class="cursor-pointer" width="20" height="6" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path fill="#000" d="M0 4h20v2H0zM0 0h20v2H0z" />
      </svg>
    </span>

    <div
      id="my-work-nav"
      class="my-work-nav-lightbox md:hidden heading-xs-uppercase fixed inset-0 w-full h-screen bg-gray-100 z-10 flex flex-col justify-center items-center text-left"
      :style="{ display: navDisplayStyle }"
    >
      <span
        class="my-work-nav-lightbox-close absolute top-0 left-0 uppercase font-bold text-xs ml-5 mt-5 z-20 cursor-pointer"
        @click="handleOpenNav"
        v-show="showCloseButton"
      >
        close
      </span>

      <!-- Sections List -->
      <ul>
        <li
          class="my-2 text-gray-500"
          @click="handleOpenNav"
          v-for="(section, sectionIndex) in sections"
          :key="sectionIndex"
        >
          <NuxtLink :to="`/gallery/${section.slug}`" class="tracking-widest text-black hover:underline">{{
            section.name
          }}</NuxtLink>

          <!-- Galleries in each section -->
          <ul>
            <li
              v-for="(gallery, index) in section.galleries"
              :key="index"
              class="my-2 ml-4 text-gray-500 hover:text-black"
            >
              <NuxtLink :to="`/gallery/${section.slug}#${section.slug}-${gallery.slug}`" class="tracking-widest">
                - {{ gallery.title }}</NuxtLink
              >
            </li>
          </ul>
        </li>

        <hr class="my-5" />

        <li>Contact Me</li>
        <li class="my-2 text-gray-500 hover:text-black">
          <a href="mailto:leal5325@live.com">Email Me</a>
        </li>

        <li class="my-2 text-gray-500 hover:text-black">
          <a href="https://www.instagram.com/seabasmedia/?hl=en" target="_blank">DM Me on Instagram</a>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'Navigation',
  data() {
    return {
      isNavOpen: this.isOpen,
      navDisplayStyle: this.isOpen ? 'flex' : 'none',
    }
  },
  methods: {
    handleOpenNav: function (e) {
      this.isNavOpen = !this.isNavOpen

      this.navDisplayStyle = this.isNavOpen ? 'flex' : 'none'
    },
  },
  props: {
    galleries: {
      type: Array,
    },
    sections: {
      type: Array,
    },
    currentSectionName: {
      type: String,
    },
    isOpen: {
      type: Boolean,
      default: false,
    },
    showCloseButton: {
      type: Boolean,
      default: true,
    },
  },
}
</script>

<style scoped>
.hide-nav {
  display: none;
}
</style>