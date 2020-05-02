<template>
  <v-layout>
    <v-flex class="text-center">
      <div id="map-wrap" style="height: 80vh">
        <no-ssr>
          <l-map :zoom=13 :center="[28.5788481,77.1389373]">
            <l-tile-layer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png"></l-tile-layer>
            <l-marker :lat-lng="[28.5788481,77.1389373]"></l-marker>
          </l-map>
        </no-ssr>
        </div>
    </v-flex>
  </v-layout>
</template>
