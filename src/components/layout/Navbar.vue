<template>
  <div id="nav">
    <nav class="navbar navbar-expand-lg navbar-light">
      <router-link to="/" class="navbar-brand">
        <img
          height="60"
          src="https://cdn3.iconfinder.com/data/icons/ballicons-reloaded-free/512/icon-70-512.png"
          alt="brand logo"
          class="pr-2"
        />
        <strong class="text-black" style="font-size: 1.4rem"
          >TrueStory</strong
        ></router-link
      >
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav ml-auto text-center">
          <li class="nav-item">
            <router-link class="nav-link" :to="{ name: 'Home' }"
              >Home</router-link
            >
          </li>
          <li class="nav-item">
            <router-link class="nav-link" :to="{ name: 'About' }">
              About</router-link
            >
          </li>
        </ul>
      </div>
    </nav>
  </div>
</template>

<style>
#nav {
  content: "";

  opacity: 0.7;

  background: linear-gradient(
    to right,
    transparent 0%,
    rgba(0, 0, 0, 0.3) 100%
  );
}
a.nav-link {
  color: red ;
}
</style>
