<script>
  import { onMount, setContext } from "svelte";
  import Explanation from "./Explanation.svelte";
  import mapbox from "mapbox-gl";
  import { syncMaps } from "./sync";
  mapbox.accessToken =
    "pk.eyJ1IjoidG1jdyIsImEiOiJjamJ6eXpkd2EwMndoMzRtb3B6NjkyZG85In0.8kzWTUAe9DL5YzckMAEYfA";

  let mapAe;
  let mapBe;
  let mapA;
  let mapB;
  let layerA;
  let layerB;

  const bounds = [
    -122.53875732421875,
    37.69169933343869,
    -122.34169006347656,
    37.8124963257741
  ];

  function pickLayer(map, name) {
    for (let l of ["zoning", "redlining", "liquefaction", 'homes']) {
      map.setPaintProperty(l, "fill-opacity", l === name ? 1 : 0);
    }
  }

  function pickLayerA(e) {
    pickLayer(mapA, e.target.value);
  }

  function pickLayerB(e) {
    pickLayer(mapB, e.target.value);
  }

  onMount(() => {
    const link = document.createElement("link");
    link.rel = "stylesheet";
    link.href = "https://unpkg.com/mapbox-gl/dist/mapbox-gl.css";

    link.onload = () => {
      mapA = new mapbox.Map({
        container: mapAe,
        style: "mapbox://styles/tmcw/ck4enq2i51om91co1x4o8zezu",
        bounds
      });
      mapB = new mapbox.Map({
        container: mapBe,
        style: "mapbox://styles/tmcw/ck4enq2i51om91co1x4o8zezu",
        bounds
      });
      mapA.removeControl(
        mapA._controls.find(c => {
          return c instanceof mapbox.AttributionControl;
        })
      );
      mapA.on("load", () => {
        pickLayer(mapA, "zoning");
      });
      mapB.on("load", () => {
        pickLayer(mapB, "redlining");
      });
      syncMaps(mapA, mapB);
    };

    document.head.appendChild(link);

    return () => {
      map.remove();
      link.parentNode.removeChild(link);
    };
  });
</script>

<style>
  #map {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
  }
  #map-a {
    position: absolute;
    left: 0;
    right: 50%;
    top: 0;
    bottom: 0;
  }
  #map-b {
    position: absolute;
    right: 0;
    left: 50%;
    top: 0;
    bottom: 0;
    border-left: 2px solid #000;
  }
  #ui {
    position: absolute;
    top: 12px;
    left: 12px;
    width: 300px;
    padding: 0px;
  }
  #ui h1 {
    font-size: 18px;
    font-weight: 800;
    margin: 0;
  }
  select {
    position: absolute;
    top: 10px;
    right: 10px;
    z-index: 999;
    font-size: 18px;
    font-family: -apple-system, BlinkMacSystemFont, avenir next, avenir,
      helvetica neue, helvetica, Ubuntu, roboto, noto, segoe ui, arial,
      sans-serif;
  }
  div.explanation {
    position: absolute;
    bottom: 40px;
    left: 0;
    right: 0;
    background: #fff;
    padding: 12px;
    z-index: 999;
  }
</style>

<div id="map-a" bind:this={mapAe}>
  <select bind:value={layerA} on:change={pickLayerA} value="zoning">
    <option value="zoning">Zoning</option>
    <option value="redlining">Redlining</option>
    <option value="liquefaction">Liquefaction</option>
    <option value="homes">Home prices</option>
  </select>
  <div class='explanation'>
    <Explanation layer={layerA} />
  </div>
</div>
<div id="map-b" bind:this={mapBe}>
  <select bind:value={layerB} on:change={pickLayerB} value="redlining">
    <option value="zoning">Zoning</option>
    <option value="redlining">Redlining</option>
    <option value="liquefaction">Liquefaction</option>
    <option value="homes">Home prices</option>
  </select>
  <div class='explanation'>
    <Explanation layer={layerB} />
  </div>
</div>
<div id="ui">
  <h1>SF Map Comparison</h1>
</div>
