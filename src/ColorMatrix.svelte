
<script>



/*

{"colorCandleDw":"#0c5b3bff","colorCandleUp":"#41a35bff","colorVolDw":"#29595555","colorVolUp":"#5ba38d3f","colorWickDw":"#0c5b3b88","colorWickUp":"#41a35b88","back":"#111519ff"}

{
    colorCandleDw: "#0c5b3bff",
    colorCandleUp: "#34a345ff",
    colorVolDw: "#29595555",
    colorVolUp: "#5ba38d3f",
    colorWickDw: "#0c5b3b88",
    colorWickUp: "#34a34588",
}


{
    colorCandleDw: "#266f78ff",
    colorCandleUp: "#469c6dff",
    colorVolDw: "#a4997973",
    colorVolUp: "#7b694aec",
    colorWickDw: "#266f7888",
    colorWickUp: "#469c6d88",
    back:"#1a1b1dff"
}

{
    "colorCandleDw": "#0c5b3bff",
    "colorCandleUp": "#41a350ff",
    "colorVolDw": "#29595555",
    "colorVolUp": "#5ba38d3f",
    "colorWickDw": "#0c5b3b88",
    "colorWickUp": "#41a35088",
    "back": "#111519ff"
}

{"colorCandleDw":"#de4646ff","colorCandleUp":"#41a376ff","colorVolDw":"#29595555","colorVolUp":"#5ba38d3f","colorWickDw":"#de464688","colorWickUp":"#41a37688","back":"#111519ff"}

// GOOD
{
    "colorCandleDw": "#0c5b3bff",
    "colorCandleUp": "#41a35bff",
    "colorVolDw": "#29595555",
    "colorVolUp": "#5ba38d3f",
    "colorWickDw": "#0c5b3b88",
    "colorWickUp": "#41a35088",
    "back": "#161c20ff"
}

*/


import HsvPicker from './ColorPicker.svelte'
import { onMount } from 'svelte'

export let chart

const Default = {
    "colorCandleUp": "#41a376ff",
    "colorCandleDw": "#de4646ff",
    "colorVolDw": "#29595555",
    "colorVolUp": "#5ba38d3f",
    "back": "#13171c",
    "llBack": "#13171c77"
}

let matrix
let colors = {
    "colorCandleDw": "#0c5b3bff",
    "colorCandleUp": "#41a35bff",
    "colorVolDw": "#29595555",
    "colorVolUp": "#5ba38d3f",
    "colorWickDw": "#0c5b3b88",
    "colorWickUp": "#41a35088",
    "back": "#111519ff"
}
colors = Default



onMount(() => {
    setTimeout(() => {
        let props = chart.hub.panes()[0].overlays[0].props
        Object.assign(props, colors)
        chart.colors = { back: colors['back'] }
        chart.update()
    },10)
})

function colorCallback(rgba, name) {
    if (chart) {
        let props = chart.hub.panes()[0].overlays[0].props
        colors[name] = rgba2rgb(rgba.detail)

        let wick = rgba2rgb(rgba.detail).slice(0, 7) + '88'

        if (name === 'colorCandleUp') {
            colors['colorWickUp'] = wick
        }
        if (name === 'colorCandleDw') {
            colors['colorWickDw'] = wick
        }
        Object.assign(props, colors)
        chart.update()
        console.log(JSON.stringify(colors))

    }
}

function onBackColor(rgba, name) {
    colors[name] = rgba2rgb(rgba.detail)
    if (chart) {
        chart.colors = { back: colors[name]}
        chart.update()
    }
    console.log(JSON.stringify(colors, null, 4))
}

function rgba2rgb(rgba)
{
    let color = '#' +
        (rgba.r | 1 << 8).toString(16).slice(1) +
        (rgba.g | 1 << 8).toString(16).slice(1) +
        (rgba.b | 1 << 8).toString(16).slice(1) +
        ((rgba.a * 255) | 1 << 8).toString(16).slice(1)

    return color
}

</script>
<style>
.color-matrix {
    display: grid;
    grid-template-columns: 50% 50%;
    width: fit-content;
    position: absolute;
    top: 0;
    right: 0;
    grid-gap: 10px;
}
</style>
<div class="color-matrix">
    <HsvPicker on:colorChange={rgba => colorCallback(rgba, 'colorCandleUp')}
        startColor={colors['colorCandleUp'].slice(0,7)}/>
    <HsvPicker on:colorChange={rgba => colorCallback(rgba, 'colorCandleDw')}
        startColor={colors['colorCandleDw'].slice(0,7)} />
    <HsvPicker on:colorChange={rgba => colorCallback(rgba, 'colorVolUp')}
        startColor={colors['colorVolUp'].slice(0,7)} />
    <HsvPicker on:colorChange={rgba => colorCallback(rgba, 'colorVolDw')}
        startColor={colors['colorVolDw'].slice(0,7)}/>
    <HsvPicker on:colorChange={rgba => onBackColor(rgba, 'back')}
            startColor={(colors['back'] || '#121417').slice(0,7)}/>
</div>
