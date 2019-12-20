<script>
  export let layer
</script>

<style>
swatch {
  width: 12px;
  height: 12px;
  display: inline-block;
  vertical-align: middle;
  margin-left: 10px;
  margin-right: 5px;
  border-radius: 6px;
}
</style>

<div>
{#if layer == "zoning"}
  <div>
  <a href='https://dcpos.ch/yimby/zoning/'>Zoning classifications and scheme borrowed from YIMBY</a>.
  <swatch style='background: #a80000' />RH-1: the burbs
  <swatch style='background: #ff7070' />RH-2 and RH-3: low density residential
  <swatch style='background: #018d40' />RM, RED, NCT, NCD: medium density
  <swatch style='background: #005eff' />C: Commercial
  <swatch style='background: #ff7605' />M, PDR: Industrial
  <swatch style='background: #c200db' />Special zones
</div>
{:else if layer == "redlining"}
<div>
  <a href='http://salt.umd.edu/T-RACES/demo/demo.html'>Redlining data from T-RACES</a>.
  <swatch style='background: #00aa00' />Area A
  <swatch style='background: #0055ff' />Area B
  <swatch style='background: #ffff00' />Area C
  <swatch style='background: #ff0000' />Area D
</div>
{:else if layer == "liquefaction"}
<div>
  <a href='https://data.sfgov.org/City-Infrastructure/San-Francisco-Seismic-Hazard-Zones/7ahv-68ap/data'>San Francisco Seismic Hazard Zones</a>.
  Areas where liquefaction and landslides may occur during a strong earthquakes.
</div>
{:else if layer == "homes"}
<div>
  <a href='https://github.com/simonkassel/Visualizing_SF_home_prices_R'>SF Home Prices, data from SF Office of the Assessor-Recorder</a>.
  <swatch style='background: #fff' />$7,000
  <swatch style='background: #800071' />$4,700,000
</div>
{:else if layer == "evictions"}
<div>
  <a href='http://www.antievictionmappingproject.net/ellis.html'>Ellis act evictions from the Anti-Eviction Mapping Project</a>.
  <div style='vertical-align: middle; margin-left: 10px; margin-right: 4px; display: inline-block;background:#bd0000;border-radius: 3px;width:3px;height:3px'/> 1 unit
  <div style='vertical-align: middle; margin-left: 10px; margin-right: 4px; display: inline-block;background:#bd0000;border-radius: 12px;width:24px;height:24px'/> 102 units
</div>
{/if}
</div>
