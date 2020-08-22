<script lang="ts">
  let bridgeTraffic = [
    { name: "Henry Hudson Bridge", traffic: 58809 },
    { name: "Hugh L. Carey Tunnel", traffic: 54007 },
    { name: "Queens-Midtown Tunnel", traffic: 86081 },
    { name: "R.F.K. Memorial Bridge Manhattan Plaza", traffic: 91509 },
    { name: "George Washington Bridge", traffic: 280784 },
    { name: "Holland Tunnel", traffic: 87232 },
    { name: "Lincoln Tunnel", traffic: 109796 },
  ];

  bridgeTraffic.sort((a, b) => b.traffic - a.traffic);
</script>

<style>
  svg {
    width: 100%;
  }
</style>

<svg>
  {#each bridgeTraffic as bridge, i}
    <g transform={`translate(220 ${i * 16 + 10})`}>
      <text y={5} font-size="12" text-anchor="end" dominant-baseline="middle">
        {bridge.name}
      </text>
      <rect x={6} height="10" width={bridge.traffic * 0.0008} fill="red" />
      <!-- Exercise: add a number at the end of each bar that shows the traffic count. -->
    </g>
  {/each}
</svg>
