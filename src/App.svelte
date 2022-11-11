<script>

import { NightVision } from 'night-vision'
import { onMount } from 'svelte'
import ColorMatrix from './ColorMatrix.svelte'

let chart // ref

// Generate some random data
function data() {
    return Array(30).fill(1).map((x, i) => [
        new Date(`${i+1} Nov 2022 GMT+0000`).getTime(),
        i * Math.random()
    ])
}

onMount(() => {
    chart = new NightVision('chart-container')
    chart.data = {
        panes: [{
            overlays: [{
                name: 'APE Stock',
                type: 'Spline',
                data: data(),
                settings: {
                    precision: 2
                }
            }]
        }]
    }
})

</script>

<div id="chart-container">
    <ColorMatrix {chart}/>
</div>
<style>

</style>
