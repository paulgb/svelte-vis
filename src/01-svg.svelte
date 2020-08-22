<!-- VS Code Svelte Extension complains if there is no script tag. -->
<script></script>

<style>
  svg {
    border: 1px solid #aaa;
    width: 100%;
    height: 600px;
  }
</style>

<svg viewBox="0 0 100 100">
  <g transform="translate(40 40)">
    <circle r="5" fill="red" />
    <g transform="translate(50 0)">
      <circle r="5" fill="blue"/>
    </g>
  </g>
  <g transform="scale(0.333)">
    <g transform="translate(30) scale(3)" fill="green">
      <circle r="5" fill="black" />
      <circle cx="40" cy="40" r="30" fill="blue" />
      <rect
        fill="none"
        x="10"
        y="20"
        width="10"
        height="10"
        stroke="white"
        stroke-width="3" />
      <line x1="30" y1="90" x2="80" y2="80" stroke="black" />
    </g>
  </g>
</svg>
