<template>
  <div class="main">
    <div class="component">
      <p style="font-size:22px; margin-bottom: 5px">Layout App</p>
      <div class="form">
        <label>Enter port for loading</label>
        <input type="text" v-model="port">
        <button @click="getRemoteComponent">Get remote component</button>
      </div>
    </div>
    <div class="component">
      <p style="font-size:22px; margin-bottom: 5px">Remote App</p>
      <component :is="dynamicComponent"></component>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      port: null,
      dynamicComponent: null
    }
  },
  methods: {
    getRemoteComponent() {
      console.log(this.port, '<- Подгружаем по порту')

      // Можно конфигурировать любые параметры динамически
      const uiApplication = {
        protocol: 'http',
        host: 'localhost',
        port: this.port,
        fileName: 'remoteEntry.js'
      }

      const remoteURL = `${uiApplication.protocol}://${uiApplication.host}:${uiApplication.port}/${uiApplication.fileName}`;
      console.log(remoteURL)

      const moduleScope = 'home'
      const moduleName = 'Content'
      const element = document.createElement('script');
      element.type = 'text/javascript';
      element.async = true;
      element.src = remoteURL;

      element.onload = () => {
        const remoteComponent = this.loadModule(moduleScope, `./${moduleName}`)
        remoteComponent.then(res => {
          console.log(res.default);
          this.dynamicComponent = res.default;
        })
      };

      element.onerror = () => {
        alert(`Port ${this.port} doesn't have any content! Try another`)
      }

      document.head.appendChild(element);
    },
    async loadModule(scope, module) {
      await __webpack_init_sharing__('default');
      const container = window[scope];
      await container.init(__webpack_share_scopes__.default);
      const factory = await window[scope].get(module);
      const Module = factory();
      return Module;
    }
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat&display=swap');

* {
  font-family: 'Montserrat', sans-serif;
  color:#fff;
}

body, p {
  margin: 0;
}

.main {
  height: 100vh;
  display: flex;
  background: gray;
}

.component {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: 2px solid #ffff;
  padding: 5px;
  border-radius: 10px;
  width: 100%;
}

.form {
  display: flex;
  max-width: 300px;
  flex-direction: column;
}

input {
  margin: 10px 0;
  color:black;
}

button {
  color: black;
}

</style>
