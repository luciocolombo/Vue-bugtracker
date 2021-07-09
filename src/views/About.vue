<template>
  <div class="container border shadow maincont">
    <h4 class="border">
      This BugTracker was created with VueJS
      <i class="fas fa-heart corazon"></i>
    </h4>
    <h6 class="border">
      Visit my site
      <a href="https://luciocolombodev.netlify.app/">Lucio Colombo</a>
    </h6>
    <img
      class="heart"
      src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fupload.wikimedia.org%2Fwikipedia%2Fcommons%2Fthumb%2F9%2F95%2FVue.js_Logo_2.svg%2F1200px-Vue.js_Logo_2.svg.png&f=1&nofb=1"
    />
  </div>
</template>


<style scoped>
.maincont {
  background: rgb(211, 211, 211);
  padding: 30px;
}
img {
  width: 500px;
  margin-top: 50px;
}

.heart {
  animation: animateHeart 1.8s infinite;
}
@keyframes animateHeart {
  0% {
    transform: scale(0.8);
  }
  5% {
    transform: scale(0.9);
  }
  10% {
    transform: scale(0.8);
    opacity: 0.5;
  }
  15% {
    transform: scale(1);
  }
  50% {
    transform: scale(0.8);
  }
  100% {
    transform: scale(0.8);
  }
}
.corazon {
  color: red;
}
</style>