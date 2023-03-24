
<template>
  <div id="cesium-container">
  </div>
</template>

<script>
import { Viewer, Ion, Cartesian3, createWorldTerrain, IonResource, ClassificationType, GeoJsonDataSource, createOsmBuildings, Cesium3DTileStyle, Cesium3DTileset} from 'cesium'
import "cesium/Source/Widgets/widgets.css";

Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI5YmFkNzBhZS0yZmNjLTQwYjYtYTE1ZC1mOTE3NjFmN2FmODMiLCJpZCI6MTI0MjEyLCJpYXQiOjE2NzYwMjY4NDB9.VPCaeRMezfo7CLvu0aaxeFwOSmxIiactJ2g3RJNf6BY'

export default {
  props: {
    showTile: {
      type: Boolean,
      default: true,
    }
  },
  name: 'CesiumViewer',
  data() {
    return {
      newBuildingTileset: null,
    }
  },
  watch: {
    showTile: {
      handler() {
        if (this.newBuildingTileset !== null) {
          this.newBuildingTileset.show = this.showTile
        }
      },
      immediate: true
    }
  },
  mounted() {
    const viewer = new Viewer(this.$el, {
      timeline: false,
      animation: false,
      terrainProvider: createWorldTerrain(), 
    });
    viewer.camera.flyTo({
      destination: Cartesian3.fromDegrees(-104.9965, 39.74248, 4000)
    });
    const buildingsTileset = viewer.scene.primitives.add(createOsmBuildings())

    viewer.scene.debugShowFramesPerSecond = true;


    // FETCH GEOJSON FROM ION AND ADD TO VIEWER
    async function addBuildingGeoJSON() {
      const geoJSONURL = await IonResource.fromAssetId(1594695)
      const geoJSON = await GeoJsonDataSource.load(geoJSONURL, { clampToGround: true })
      const dataSource = await viewer.dataSources.add(geoJSON)
      for (const entity of dataSource.entities.values) {
        entity.polygon.classificationType = ClassificationType.TERRAIN;
      }
      viewer.flyTo(dataSource)
    }
    addBuildingGeoJSON()

    // REMOVE GEOJSON FROM VIEWER
    if (buildingsTileset.style._ready) {
    buildingsTileset.style = new Cesium3DTileStyle({
      show: {
        conditions: [
          // Any building that has this elementId will have `show = false`.
          ['${elementId} === 532245203', false],
          ['${elementId} === 332469316', false],
          ['${elementId} === 530288180', false],
          ['${elementId} === 235368665', false],
          ['${elementId} === 332469317', false],
          
          // If a building does not have this elementId, show it.
          [true, true]
        ]
      },
      color: "Boolean(${feature['cesium#color']}) ? color(${feature['cesium#color']}) : color('white')"
    })
    }

    // ADD THE NEW BUILDING TO THE VIEWER
    this.newBuildingTileset = viewer.scene.primitives.add(
      new Cesium3DTileset({
        url: IonResource.fromAssetId(1595164),
      })
    );
    viewer.flyTo(this.newBuildingTileset)

  }
}
</script>

<style scoped>
#cesium-container {
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
}
</style>