
<template>
  <header>
    <div class="header-container">
      <img :src="require(`../assets/images/logo.png`)">
    </div>
</header>
</template>


<style>
  header{
    width: var(--site-width);
    margin: 4rem auto;
    
  }

  .header-container{
    width: 100%;
  }

  .header-container img{
    margin: 0 auto;
  }
</style>
