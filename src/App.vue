<template>
  <div id="nav">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">第六週作業</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
          <div class="navbar-nav">
            <router-link to="/" class="nav-link">首頁</router-link>
            <router-link to="/productlist" class="nav-link">產品列表</router-link>
            <router-link to="/cart" class="nav-link">購物車</router-link>
            <router-link to="/backstage" class="nav-link">後台</router-link>
          </div>
        </div>
      </div>
    </nav>
  </div>
  <div class="container">
    <router-view/>
  </div>
  </template>

<style lang="scss">
@import "bootstrap"
</style>
