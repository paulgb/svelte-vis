<script lang="ts">
  export let position: { x: number; y: number };

  let lastX = 0;
  let lastY = 0;

  function mousedown(c) {
    lastX = c.screenX;
    lastY = c.screenY;
  }

  function mousemove(c) {
    if (c.buttons) {
      let deltaX = c.screenX - lastX;
      let deltaY = c.screenY - lastY;
      lastX = c.screenX;
      lastY = c.screenY;

      position = {
        x: position.x + deltaX,
        y: position.y + deltaY,
      };
    }
  }
</script>

<g
  on:mousedown={mousedown}
  on:mousemove={mousemove}
  on:mouseout={mousemove}
  transform={`translate(${position.x} ${position.y})`}>
  <circle r={20} opacity="0.05" />
  <circle r={8} fill="#800" />
</g>
