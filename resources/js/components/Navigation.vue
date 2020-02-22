<template>
  <nav class="flex items-center justify-between flex-wrap bg-blue-500 p-6">
    <div class="flex items-center flex-shrink-0 text-white mr-6">
      <a href="/" class="font-semibold text-xl tracking-tight">Larticles</a>
    </div>
    <div class="block lg:hidden">
      <button
        class="flex items-center px-3 py-2 border rounded text-blue-200 border-blue-400 hover:text-white hover:border-white"
      >
        <svg class="fill-current h-3 w-3" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
          <title>Menu</title>
          <path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z" />
        </svg>
      </button>
    </div>
    <div class="w-full block lg:flex lg:items-center lg:w-auto">
      <div class="text-sm lg:flex-grow">
        <a
          href="login"
          class="block mt-4 lg:inline-block lg:mt-0 text-blue-200 hover:text-white mr-4"
        >Login</a>
        <a
          href="register"
          class="inline-block text-sm px-4 py-2 leading-none border rounded text-white border-white hover:border-transparent hover:text-blue-500 hover:bg-white mt-4 lg:mt-0"
        >Register</a>
      </div>
    </div>
  </nav>
</template>
