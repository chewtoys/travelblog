<template>
  <!--
  http://www.smashingmagazine.com/2015/09/making-svg-maps-from-natural-earth-data/
  https://codepen.io/janez-svetin/pen/epBebN?editors=1000
  -->
  <section id="map" class="map">
    <div class="container narrow">
      <h1>{{ blok.headline }}</h1>
      <p>Das sind jene Länder, in denen wir <strong class="visited">gewesen sind.</strong></p>
      <!--<p>Das sind alle Länder, in denen wir <strong class="visited">schon gewesen sind</strong> oder in die wir noch <strong class="planned">vorhaben zu reisen.</strong></p>-->
      <div class="map__illustration">
        <span v-bind:style="{ top: this.yPosition + 'px', left: this.xPosition + 'px' }" v-bind:class="{ active: hover }" class="map__tooltip">{{ this.country }}</span>
        <div class="map__active" v-bind:style="{ top: blok.active_position_top + '%', left: blok.active_position_left + '%' }">
          <p>Wir sind wieder in Österreich</p>
          <!--<p>Wir sind gerade in <strong class="active">{{ blok.city }}</strong></p>-->
          <svg><use xlink:href="#curved-arrow"></use></svg>
        </div>
        <svg @mouseover="showTooltip" @mousemove="updateCoordinates" @mouseleave="hideTooltip" version="1.1" xmlns="http://www.w3.org/2000/svg" x="0" y="0" viewBox="0 0 252.2 346.8" xml:space="preserve">
          <path id="US" title="USA" d="M8.7 6.3l.4 3.4-.7 1.1L7 6.9 3 5.4l1.3 6.2.5 5.4-.7 7.1L3 27.7l1 2.9.1 3.2-.6 3 1.5 2 .4 2.9 3.3 4.9 2.3 4.8 2.7 3.4 7.8 5.2 1.8 1.9.5 1.7 6.7-.6-.3.6 10.5 3.9h7.7v-1.3h4.8l4.1 3.6.9 1.7 4.4 3.7 1.7-2.5 2.3-.1 2 1.3 4 5.8 1.4 3.6 4.1 1.6 1.1-.1-.6-4.7 2.2-2.9 2.8-1.4 2.5-2.4 8.5-.6 2.1 1.7 2 .1 2.1-2.5 2.8-1.6 2.5.4 3.1-.4 1.7.8 4.6.2 1.1.5 2.9 4.4-.6 2.1 3.2 6.3 2.9 2.4.8-.4.9-5.2-1.3-5-2.6-7.2.4-2.3 2.9-3.5 8.1-6.7 2.9-1 1.8-2.5-1.5-4.9-.8-6.2.5-1.5.3 3 1.4 1.3-.9 2.4 2.7-4-1.3-3.9 1.7 2 2-2.8.6-2.6 5.6-1.8-.8-.7-4.1.7 2.3-1.1 6.6-2.2-.9-2 2-5.1 2.9-1.1 5.8-3.2-.5-1.3-1.8-2.2v-5.5l-1.2-1.2-1.9.7-.9-1.1-2.1 3.1-.8 3.1-3.1 2.6-.3 1h-9.3l-4.1 3.4.3.5.2 1.5-2.1 1.3-4.5-.5-1.3.4.2 1.1.1.4-2.4 2.2-5.2 2.6-2 .5-3.1-1 1-1.4.6-1.3 1.3-2.1-.6-3.8-3.4.3 1.5-1.5v-3.6l-2.1-1.5-2.4-.8-1.5 3.8-.3-1.3-2.5 4.2.8 4-.1 2.5-1.1 1.8-2.3.9-1.3-6 1.2-4.9 1.4-3-2.5 2.5-.4-.5 2.7-4 4-1.2 4.7.1-1.4-1.6-.7-.1-1.4-1.3-5.2 1.3-3.5-2.1.9-2.2-6.8 3.7-1.7-1.4-2.8.8 3.9-4L100 7l-3.4-1.1-2.2.5-2.7-1.3L86 3.5l-.5-2.3-.9 1.7H7.8l.9 3.4z"/>
          <path id="BR" title="Brasilien" d="M191 154l-4.9 1.7-1.4-.1-2.6-4 1.2-3.3-1.9-3.6-1.4.1.4.8-1 1.1-5.1 1.5-.8.7-4.8-.8 1.2.7.8 5.2-3.5 2.3-2.7 1-1.5-1.5-.5.3-2.2-1.6-5.4-.1v1.7l1.6.3-2.2 1.2v2l1.6 3.8-1.2 7.6-2.5-.1-5.8 2.8-.6 3.8-1.7.8-.7 1.7 2.7 4.2-.6 1.2 1.8.2 1 1.5 2.4.1 2.3-1.7-.2 4.3 1.3.3 1.6-.5 2.1.2 1.4-.1 4.5-3.1 3.6-.5-.2 5.1 3 2.5 3.1.5 4.1 2.5 3.4.8.7 3.7h-.8l1.1 3.4 5.3.1 2.1 5.4-1 5.3-.9.6.8 1.6-.2 4 2.9.6 1.1-.6 4 5.8 1-.5.6 4-.5 1.8 1.4-.6 1.4 1.8-.1 2.5-7.3 6.1-3.7 4.3 1.8-.3 2.8 2.5 1-.1 6.6 6.1-1.2 1.6.8 1.9 1.8-1.9 1.3-3.1 4.3-4.1 3.1-5.6 3.1-6.4-.1-4 2.3-3 3.3-2.4 3.1-.9 2-1.3 4.4-1.2h3l.6-1.8 2.2-1.3.5-3.1 2.8-3.9.5-3.9 1.9-7.5-.2-5.4 3.5-4.6 1.8-3.2 5.3-5.8 1.1-4.6-1.4-5.2-1-.9-4.5-.9-3.5-3.1-4.1-2.3-4.1.1-5.4-1.5-3.2.9.5-1.5-1.4-1.6-8.1-2.7-2.1 1.8-.1-2.8-4.9-.4-.9-.8 2.1-2.3-.1-1.9-1.5-.5-2.2-6.4-.9.1-3.6 5.6-1.3.2-1.9-.1-1.2-.6-1.6-.6-1.3.3-1.1-.2-.1 1.9-1.5-.2-.6.1z"/>
          <path id="PY" title="Paraguay" d="M187.1 215.4l-2.6-1.5h-2.6l-4.8.9-1.4 4.2-1.1 3.6 5.1 4.9 2.3.5 3.4 2.2 2.9 1.2.4 1.4-2.7 4.7 5.9 1.3 2.2-.5 2.5-2.4.5-2.7.5-1.8-.6-4-1 .5-4-5.8-1.1.6-2.9-.6.2-4-.8-1.6-.3-1.1z"/>
          <path id="UY" title="Uruguay" d="M188.7 247.2l-.7 2.6-.7 3.3-.8 6.2 1.7 1.9 1.9-.1 2.6 1.4 3.5.3 3.1-1.9 1.2-2.1-.8-1.9 1.2-1.6-6.6-6.1-1 .1-2.8-2.5-1.8.4z"/>
          <path id="VE" title="Venezuela" d="M181.7 135.2l-1.4.1-.4-2.2-2.1-1.4-2.2-.2-.9-1.3 2.4-.8-6.8.2-1.6 1.6-2.1-.3-1.6-1.3-5.5.3-1.9-2.5h-1.9l-1-2-.6 2.2-3.5 1.1 1 3.1-.6 2-1.2.2-1-2.2.4-4.4 1.6-.3.1-.7-1.8.5-2.6 3.3-1.1 3.7 2.4 2.1 1.3 4 5.2.1 2 2.4 4.7-.5 1 .5-1.3 4.4 1.4 3.3-1.4 1.4 1.7 1.6.8 2.8 1.5 1.5 2.7-1 3.5-2.3-.8-5.2-1.2-.7 4.8.8.8-.7 5.1-1.5 1-1.1-.4-.8-1.9-2.1.8-.8-.1-1.3 3.9-4.7-1.2-.9z"/>
          <path id="SV" title="El Salvador" d="M100.7 118.7l-.6.2-1.3 1.4-.1.4.8.6 1.5.2 3.8.9.3-.7-.8-1.7-1.2.3-1.5-1.4-.9-.2z"/>
          <path id="AR" title="Argentinien" d="M158.1 331.5V342h3l1.7.1 1.4 1.7 2.6-.2 1.2-2.4-3.9-1.2-3.6-2.8-1.4-3.5-1-2.2zm4.3-107.5l.3.8-1 3.1-3 1.5.3 7.3-2 1.9-1.8 2.9-1 2.9.3 3.1-1.7 3.3 1.3 5.7.7.6v3.1l-1.6 3.3.1 2.8-2.1 2.2v3.2l.8 3.4-1.7 1.3-1.4 6.9.5 4.5-1.1.8.6 4.4 1.3 1.5-.9 1.6.7 5.3-1 5.3-1.5 3.5.3 2.1-.8 2.7-2.1 1.9.2 4.6 1 1.6 1.8-.3-.1 3.3 1.1 2.6 6.7.6 2.6.7 1.2.2-1.8-2.6-.9-4.6 1.1-2 2.5-1.7 1.8-5 3.3-2.4 1-3.7-2.7-.8-2.7-3 .8-3 2.2-2h2.6l1.1-6 2.2-2.4 2.6-1.2-.8-1.9-1.5 1.2-1.9-1.1-.4-3.7 1.1-1 2.7 1.3 2.8-.5 1.7-1.3-.5-1.8.6-2.7-.6-2.1 3 .4 5.6-.7 4.1-1.9 2.7-4.5.1-1.7-1.7-1.5.4-2.4-3.5-2.9.2-1.7.8-6.2.7-3.3.7-2.6 3.7-4.3 7.3-6.1.1-2.5-1.4-1.8-1.4.6-.5 2.7-2.5 2.4-2.2.5-5.9-1.3 2.7-4.7-.4-1.4-2.9-1.2-3.4-2.2-2.3-.5-5.1-4.9-.4-.6-3.2-.1-1.1 2.4-1.6-2.2-3.6-.7-2.3 2.7z"/>
          <path id="BS" title="Bahamas" d="M133.5 91.3l-1.8-4.4-.6 1.9 2.4 2.5zm-.8-8.6l-3.2-.7.2 1.1 3-.4zm2.2-.1l-.9.2v1.6l.9-1.8z"/>
          <path id="BZ" title="Belize" d="M102.4 113.5l1-.9.2-3.2v-2.5h-.5l-1.8 1.6V111.2l-.2 3.3h.8l.5-1z"/>
          <path id="BO" title="Bolivien" d="M188 215.7l1-5.3-2-5.4-5.3-.1-1.1-3.4h.8l-.7-3.7-3.4-.8-4.1-2.5-3.1-.5-3-2.5.2-5.1-3.6.5-4.5 3.1-1.4.1-2.1-.2 2.4 4.6-.8 5.4-1.1 1.4-.1 2 1.2 2.4-1.7 3.1 1.4 2 .4 2.1 1.5 1.2-.9 2.9 1.5 3.3 1.1 4.1 2-.4 2.3-2.7 3.6.7 1.6 2.2 1.1-2.4 3.2.1.4.6 1.1-3.6 1.4-4.2 4.8-.9h2.6l2.6 1.5v.9l.7-.5z"/>
          <path id="CL" title="Chile" d="M162.8 342.1l-1.7-.1h-3v-10.5l-2-.5-2.6 1.9-.9 3.2-1.4 2.2-3.7-1.7-3.9-3.1-2.3-1 3.8 5.2 6.3 5.2 2.9.7 5 2 2.4-1.5 1.1-2zm-7.3-133.4l-.7 1.5-1.4.7.6 4.1.2 4.8-1.8 13.1-.5 6-1.6 3.8.3 3.9-.8 2.7.6 4.9-1.2 4.9-3.6 11-1.2.1.2 4 .8 3.5-1.3 2.5-1.8 12.2 1.7.5.9-4.7 1.9 1-1.4 7.9-3.1-1.4-.9 6.5-2.6 3.5 4.2 1.2-2.9 3.2-1.2 4 .4 7.3 1.4 2.9-.8 2.6.9 2.8 6.6 5.8 3.1 1.5 1.2-.1.4-4.3 3.8-2.8h2.5l-2.6-.7-6.7-.6-1.1-2.6.1-3.3-1.8.3-1-1.6-.2-4.6 2.1-1.9.9-2.7-.3-2.1 1.5-3.5 1-5.3-.7-5.3.9-1.6-1.3-1.5-.6-4.4 1.1-.8-.5-4.5 1.4-6.9 1.7-1.3-.8-3.4v-3.2l2.1-2.2-.1-2.8 1.6-3.3v-3.1l-.7-.6-1.3-5.7 1.7-3.3-.3-3.1 1-2.9 1.8-2.9 2-1.9-.3-7.3 3-1.5 1-3.1-.3-.8-2 .4-1.1-4.1-1.5-3.3.9-2.9-1.5-1.2-.4-2.1-1.6-1.9z"/>
          <path id="CO" title="Kolumbien" class="visited" d="M151.2 125.3l-1.7-.9-4.6 3.4-2.2-.2-3.6 2-1.7 3.6-2.1 2-1.3-.2.3 2.1-.5.8-1.2 1.2 1.1 1.5 1 7.9-2.2 3.2-1.4.2-1.6 2.6.4.9 4 2.7 2.4.4 2.1.5 1.2.7.7-.3 4 3.3 1.6 2.9 2.1.3 4.2-.5 2.1 1.3-1.8 2.8.8.1 1.4 1.5 1.2-7.6-1.6-3.8v-2l2.2-1.2-1.6-.3v-1.7l5.4.1 2.2 1.6.5-.3-.8-2.8-1.7-1.6 1.4-1.4-1.4-3.3 1.3-4.4-1-.5-4.7.5-2-2.4-5.2-.1-1.3-4-2.4-2.1 1.1-3.7 2.6-3.3 1.8-.5.5-1z"/>
          <path id="CU" title="Kuba" d="M127.5 94.3l-2.6-1-10 1.6-.6 1.1 2.6.1 2-1.6 2.7.2-.1 1.3 3.6.5 3.4 1.4 1.6-.1 4.5 3.5-1.9 1.6 9.6-.6.3-.7-6.5-2.7L131 95l-2.6.3-.9-1z"/>
          <path id="DO" title="Dominikanische Republik" d="M149.8 109.1l.7.5 6.2-2.4 1.3.6 1-1.2-2.5-2.1-1.5.1-2.9-1.7h-2.2l-.3.5v2.7l-.7.5.7 1.7.2.8z"/>
          <path id="FK" title="Falklandinseln" d="M186.1 324.6l-1.7 1.8-2.4-1.1-3.3 2.7 1.4 2 2.4-2 1.2 1.6 3.7-1.4.8-1.6-2.1-2z"/>
          <path id="GF" title="Französisch Guyana" d="M204.8 146.5l-2.9-2.3-2-.7-.9-.3-1.4 2.4.2 1.9 1.1 1.6-.5 1.2-.2 1.3-.7 1.2 1.2.6 1.9.1 1.3-.2 3.6-5.6-.7-1.2z"/>
          <path id="GT" title="Guatemala" d="M98.7 108.9h-2.6v1.6l1.5 3.4H94l-1.3 2.3v2l1.5 1.2 1.3.6 1.7.1 1.4.5.1-.4 1.3-1.4.6-.2.6-.7 1.3-1.9 1.3-1.1-.8-.4-.2.4-.9-.5h-.8l.2-3.3v-2.3l-2.6.1z"/>
          <path id="GY" title="Guyana" d="M188.9 141.6l-2.6-2.9-1.7-1.8-1.8-1-3.9 4.7.1 1.3-.8.8 1.9 2.1 1.4-.1 1.9 3.6-1.2 3.3 2.6 4 1.4.1 4.9-1.7.7-.1-1.7-2.4-2.5-3.6 2-2.8.4-2.5-1.1-1z"/>
          <path id="HN" title="Honduras" d="M117.2 116.3l-1.8-1.1-2.6-1-8.7.9-.3-.1-1.3 1.1-1.3 1.9-.6.7.8.2 1.5 1.4 1.2-.3.8 1.7.8.2.5.9 1.6-.8-.1-1.4 2.6-.2 3.7-2.2 3.6-1.1-.4-.8z"/>
          <path id="HT" title="Haiti" d="M147.2 102.9l-2.3.7 1.7.5 1.2 2.4-1 .7-4.6-.6 1.2 1.9 4.3-.5 1.8.5-.7-1.7.7-.5v-2.7l-2.3-.7z"/>
          <path id="JM" title="Jamaika" d="M135.2 108.8l1.5-.9-1.5-.7-3.7-.2-.3.7 4 1.1z"/>
          <path id="MX" title="Mexiko" class="visited" d="M24.8 66.6l3.3 6.7 3.8 3.1-1.2 2.6.3 1.9 6 3.5.3 3.9 5.2 4 .7 1.8-.4-4.4-1.3-.1-2.7-7.3-3.2-3.5-2.3-4-2.1-2.2-1.7-5.3.4-1.3 4.5 2 .9 3.7 4.5 6.6 1.5.3.7 2.2 3.1 2.2-.1 2 2.9 2 1.3 1.9 2.8 2.4 4.6 7-.4 2.6-.9.3 2.1 3.3 3 1.7 1.2 1.3 2.8.9 16.4 6.7 3.6-1.4 5.2 1.5 3.1 3.1v-2L94 114h3.6l-1.5-3.4V109h5.2v-.4l1.8-1.6h1.1l.7.8 2.7-7.6.1-1.4-4.8-.5-4.8 1.5-1.4 5.1-1.8 1.2-8.4 2.1-1.1-1.2-2.9-.8-3.6-5.3-1.6-6.9.2-4.1 1.6-4.9-1.1-.1-4.1-1.6-1.4-3.6-4-5.8-2-1.3-2.3.1-1.7 2.5-4.4-3.7-.9-1.7-4.1-3.6h-4.8v1.3h-7.7l-10.5-3.9.3-.6-6.7.6 1.1 3z"/>
          <path id="NI" title="Nicaragua" d="M106.1 123.1h-.8l3.2 3.1 1.3 1.1 1 .9 5 1 .7-.6-.4-.5.9-3.7-.1-3.3.8-3.8.2-.3-3.6 1.1-3.7 2.2-2.6.2.1 1.4-1.6.8-.4.4z"/>
          <path id="CR" title="Costa Rica" class="visited" d="M110.1 128.8l.8 2.7 2.6.1.2.8 3.2 3.3 1.5.6.3-1.6.4-.3.5-1.8-1.3-1.2-1.1-1.1-.7-1.5-.7.6-5-1-.7.4z"/>
          <path id="PA" title="Panama" class="visited" d="M132.9 134.3l-5.1-1.9-.9.8-6.4.9.1-.6-1-1-.5 1.8-.4.3-.3 1.6.3.4 5 .7.5 1.7 2.4-.9-1.3-1.5 3.8-2.5 1.4.8 1.1 3.4.9.8 1.2-1.2.5-.8-.2-2.1-1.1-.7z"/>
          <path id="PE" title="Peru" class="visited" d="M153.3 169.6l-.8-.1 1.8-2.8-2.1-1.3-4.2.5-2.1-.3-1.6-2.9-4-3.3-.7.3.4 2.1-.9 1.8-3 2.9-3.3 1.1-2.2 4.3-1.6 1.1-3.4-1.5.4-2.8-2.2 1.8-.9 1.9 1.3 2.7-.9 1.2 4.1 3 2 3.3 5.4 10.8 2.3 3.7-.5.8 1.1 2.4 2.1 1.8 5 3.2 5.5 2.9.2 1.2 2.8 1.7 1.4-.7.7-1.5 1.7-3.1-1.2-2.4.1-2 1.1-1.4.8-5.4-2.4-4.6-1.6.5-1.3-.3.2-4.3-2.3 1.7-2.4-.1-1-1.5-1.8-.2.6-1.2-2.7-4.2.7-1.7 1.7-.8.6-3.8 5.8-2.8 2.5.1-1.2-1.8z"/>
          <path id="EC" title="Ecuador" class="visited" d="M138.3 158.9l-2.1-.5-2.4-.4-4-2.7-1.9 1.1-3.9 5.7 1.6 4.5 1.1-1.3.6 1.2-1.5 2.1-.4 2.8 3.4 1.5 1.6-1.1 2.2-4.3 3.3-1.1 3-2.9.9-1.8-.4-2.1-1.1-.7z"/>
          <path id="PR" title="Puerto Rico" d="M166.6 107.7l-4.2-.9-.2 1.7 4.4-.8z"/>
          <path id="SR" title="Suriname" d="M190 142.6l-.4 2.5-2 2.8 2.5 3.6 1.7 2.4 1.5.2.1-1.9 1.1.2 1.3-.3 1.6.6.7-1.2.2-1.3.5-1.2-1.1-1.6-.2-1.9 1.4-2.4-3-.7-2.2.2-.3.5-3.4-.5z"/>
          <path id="TT" title="Trinidad and Tobago" d="M179.5 131l-2.1-1.8.1 1.1 2 .7z"/>
        </svg>
      </div>
    </div>
    <div class="timeline">
      <div>
        <component :key="blok._uid" v-for="blok in blok.timeline.slice().reverse()" :blok="blok" :is="blok.component"></component>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  data () {
    return {
      country: '',
      hover: false,
      xPosition: 0,
      yPosition: 0
    }
  },
  methods: {
    showTooltip (event) {
      if(event.target.attributes.title) {
        this.hover = true
        this.country = event.target.attributes.title.value
      }
      else {
        this.hover = false
        this.country = ''
      }
    },
    hideTooltip (event) {
      this.hover = false
    },
    updateCoordinates (event) {
      this.xPosition = event.clientX + 10
      this.yPosition = event.clientY + 10
    }
  },
  props: ['blok']
}
</script>

<style lang="scss" scoped>
$visited-fill: #D9F7ED;
$visited-stroke: #3FD4A2;
$planned-fill: #CEE5F7;
$planned-stroke: #4E9CDF;
$unvisited-fill: #FFF;
$unvisited-stroke: #E0E0E7;
$active: #EA4236;

.map {
  background: #F2F6FA;
  padding: 70px 0 0;
  position: relative;
  text-align: center;
  overflow: hidden;

  h1 {
    margin-top: 0;
  }

  p {
    font-size: 1.8rem;

    .visited {
      color: $visited-stroke;
    }

    .planned {
      color: $planned-stroke;
    }

    .active {
      //color: $active;
    }
  }
}

.map__illustration {
  margin: 30px 0 0;
  position: relative;
  display: inline-flex;

  > svg {
    display: block;
    margin: 0 auto;
    width: 400px;
    max-width: 100%;
    filter: drop-shadow(0 50px 100px rgba(#607E95,0.2));

    path {
      fill: $unvisited-fill;
      stroke: $unvisited-stroke;
      stroke-width: 0.8;
      cursor: pointer;
      transition: all 0.2s ease;

      &:hover {
        //fill: darken($unvisited-fill, 2%);
      }

      &.visited {
        fill: $visited-fill;
        stroke: $visited-stroke;

        &:hover {
          fill: darken($visited-fill, 5%);
        }
      }

      &.planned {
        fill: $planned-fill;
        stroke: $planned-stroke;

        &:hover {
          fill: darken($planned-fill, 5%);
        }
      }
    }
  }
}

.map__active {
  width: 10px;
  height: 10px;
  border-radius: 100%;
  position: absolute;
  z-index: 1;
  transform: translate(-50%, -50%);
  pointer-events: none;
  top: 29% !important;
  left: 42% !important;
  p {
    position: absolute;
    margin: 0;
    width: 150px;
    left: 60px;
    bottom: 30px;
  }
  svg {
    width: 28px;
    height: 32px;
    position: absolute;
    fill: #343434;
    bottom: -10px;
    left: 140px;
    transform: scaleX(-1);
  }
}

/*.map__active {
  width: 8px;
  height: 8px;
  border-radius: 100%;
  position: absolute;
  z-index: 1;
  background: $active;
  box-shadow: 0 0 0 8px rgba($active, 0.3);
  animation: animation-pulse 2s infinite;
  transform: translate(-50%, -50%);
  pointer-events: none;

  p {
    position: absolute;
    margin: 0;
    width: 140px;
    right: 10px;
    bottom: 40px;
  }

  svg {
    width: 28px;
    height: 32px;
    position: absolute;
    fill: #343434;
    bottom: 4px;
    right: 18px;
    transform: scaleX(-1);
  }
}*/

.map__tooltip {
  position: fixed;
  pointer-events: none;
  background: #343434;
  color: #fff;
  border-radius: 4px;
  padding: 5px 10px;
  font-weight: 500;
  display: none;
  z-index: 2;

  &.active {
    display: block;
  }
}

@keyframes animation-pulse {
  0% {
    box-shadow: 0 0 0 0 rgba($active, 0.4);
  }
  70% {
    box-shadow: 0 0 0 8px rgba($active, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba($active, 0);
  }
}
</style>
