<template>
  <div id="app">
    <h1>Vue BimFace Example</h1>
    <div id="bimface" style="width: 100%; height:800px"></div>
  </div>
</template>

<script>
import {} from "../static/jssdk/BimfaceSDKLoader@latest-release";
export default {
  name: "App",
  data() {
    return {};
  },
  mounted() {
    this.show_bimface();
  },
  methods: {
    show_bimface() {
      var viewer;
      var BimfaceLoaderConfig = new BimfaceSDKLoaderConfig();
      BimfaceLoaderConfig.env = BimfaceEnvOption.Local;
      //BimfaceLoaderConfig.sdkPath = "../static/jssdk";
      BimfaceLoaderConfig.resourcePath = "https://rocmos.cn/V3.0.0/company_static_files/company_1/0cfdacf6b0169b7d6bc84f6d39db2968/viewToken.json";
      BimfaceSDKLoader.load(
        BimfaceLoaderConfig,
        onSDKLoadSucceeded,
        onSDKLoadFailed
      );

      function onSDKLoadSucceeded(viewMetaData) {
        if (viewMetaData.viewType == "3DView") {
          var view = document.getElementById("bimface");
          var config = new Glodon.Bimface.Application.WebApplication3DConfig();
          config.domElement = view;
          var eventManager = Glodon.Bimface.Application.WebApplication3DEvent;
          let app = new Glodon.Bimface.Application.WebApplication3D(config);
          let viewer = app.getViewer();
          viewer.addModel(viewMetaData);
        } else if (viewMetaData.viewType == "rfaView") {
          var view = document.getElementById("bimface");
          var config = new Glodon.Bimface.Application.WebApplicationRfaConfig();
          config.domElement = view;
          var eventManager = Glodon.Bimface.Application.WebApplicationRfaEvent;
          let app = new Glodon.Bimface.Application.WebApplicationRfa(config);
          let viewer = app.getViewer();
          viewer.addModel(viewMetaData);
        }
      }

      function onSDKLoadFailed(error) {
        console.log("Failed to load SDK!");
      }
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
