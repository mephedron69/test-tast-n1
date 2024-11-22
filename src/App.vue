<template>
  <div class="main">
    <div class="main-router">
      <router-view/>
    </div>
  </div>
</template>

<style lang="scss">
#app {
  width: 100vw;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: stretch;
}

html,body,p, h1, h2, h3, h4, h5, h6 {
    overflow-x: hidden;
    margin: 0 ;
    padding: 0 ;
}

.main {
    background: white;
    min-height: 100vh;
    min-width: 100vw;
    display: flex;
    flex-direction: column;
    align-items: stretch;
    &-router {
        flex-grow: 1;
    }
}
</style>
