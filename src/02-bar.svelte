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
</script>

<style>
  svg {
    border: 1px solid #ddd;
    width: 100%;
    height: 500px;
  }
</style>

<svg>
  {#each bridgeTraffic as bridge, i}
    <g transform={`translate(300 ${i * 20 + 10})`}>
      <rect width={bridge.traffic / 1000} height="10" fill="red" />
      <text font-size="12" text-anchor="end" dominant-baseline="middle" x="-5" y="5">{bridge.name}</text>
    </g>
  {/each}
</svg>
