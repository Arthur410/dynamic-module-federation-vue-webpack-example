<template>
  <main class="main">
    <h3>Host App</h3>
    <Content />
  </main>
</template>

<script>
import { ref, defineAsyncComponent } from "vue";
export default {
  components: {
    Content: defineAsyncComponent(() => import("./components/Content")),
  },
  setup() {
    const count = ref(0);
    const inc = () => {
      count.value++;
    };

    return {
      count,
      inc,
    };
  },
};
</script>

<style>
/* Немного стилей */
@import url('https://fonts.googleapis.com/css2?family=Montserrat&display=swap');

img {
  width: 200px;
}
h1 {
  font-family: Arial, Helvetica, sans-serif;
}

html,body {
  margin: 0;
}

h3 {
  margin: 0;
  color:#d9c1e4;
}

.main{
  height: 100vh;
  background: gray;
  display: flex;
  flex-direction: column;
  justify-content: center;
  font-family: 'Montserrat', sans-serif;
  align-items: center;
  color: #fff;
}
</style>
