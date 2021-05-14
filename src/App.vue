<template>
    <div id="app">
        <div id="block">
            <div style="position:relative">
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="200"
                    height="200"
                >
                    <g>
                        <title>Layer 1</title>
                        <rect
                            v-for="elem in areas"
                            :key="elem.id"
                            :id="`svg_${elem.id}`"
                            :height="elem.h"
                            :width="elem.w"
                            :y="elem.y"
                            :x="elem.x"
                            :fill="elem.fill"
                            @drop="onDrop($event, elem.id)"
                            @dragover.prevent
                            @dragenter.prevent
                        />
                    </g>
                    <text
                        xml:space="preserve"
                        text-anchor="start"
                        font-family="sans-serif"
                        font-size="24"
                        stroke-width="0"
                        id="svg_2"
                        y="49.76"
                        x="37.34"
                        stroke="#000"
                        fill="#000000"
                    >
                        A
                    </text>
                    <text
                        xml:space="preserve"
                        text-anchor="start"
                        font-family="sans-serif"
                        font-size="24"
                        stroke-width="0"
                        id="svg_6"
                        y="52"
                        x="140.1"
                        fill="#000000"
                    >
                        B
                    </text>
                    <text
                        xml:space="preserve"
                        text-anchor="start"
                        font-family="sans-serif"
                        font-size="24"
                        stroke-width="0"
                        id="svg_7"
                        y="153.76"
                        x="34.34"
                        fill="#000000"
                    >
                        C
                    </text>
                    <text
                        xml:space="preserve"
                        text-anchor="start"
                        font-family="sans-serif"
                        font-size="24"
                        stroke-width="0"
                        id="svg_8"
                        y="155.86937"
                        x="141.66031"
                        fill="#000000"
                    >
                        D
                    </text>
                </svg>
                <div
                    v-for="(elem, index) in buttons.filter((x) => x.locateId)"
                    class="circle unselectable"
                    draggable="true"
                    @dragstart="onDragStart($event, elem)"
                    :key="index"
                    :style="{
                        left: `${elem.left}px`,
                        top: `${elem.top}px`,
                    }"
                >
                    {{ elem.value }}
                </div>
            </div>
            <div
                v-for="(elem, index) in buttons.filter((x) => !x.locateId)"
                draggable="true"
                class="circle unselectable"
                @dragstart="onDragStart($event, elem)"
                :key="index"
                :style="{ left: `${elem.left}px`, top: `${elem.top}px` }"
            >
                {{ elem.value }}
            </div>
        </div>
    </div>
</template>

<script>
import { arraysEqual } from "@/services/index";
export default {
    name: "App",
    data() {
        return {
            buttons: [
                {
                    value: 1,
                    left: 0,
                    top: 0,
                    areaId: [1, 2],
                },
                {
                    value: 2,
                    left: 275,
                    top: 0,
                    areaId: [1, 2],
                },
                {
                    value: 3,
                    left: 0,
                    top: 275,
                    areaId: [3, 4],
                },
                {
                    value: 4,
                    left: 275,
                    top: 275,
                    areaId: [3, 4],
                },
            ],
            areas: [
                {
                    id: 1,
                    h: 100,
                    w: 100,
                    x: 0,
                    y: 0,
                    fill: "#FFF093",
                },
                {
                    id: 2,
                    h: 100,
                    w: 100,
                    x: 100,
                    y: 0,
                    fill: "#84D3DB",
                },
                {
                    id: 3,
                    h: 100,
                    w: 100,
                    x: 0,
                    y: 100,
                    fill: "#8CD79F",
                },
                {
                    id: 4,
                    h: 100,
                    w: 100,
                    x: 100,
                    y: 100,
                    fill: "#AF84A3",
                },
            ],
        };
    },
    methods: {
        onDragStart(e, element) {
            e.dataTransfer.dropEffect = "move";
            e.dataTransfer.effectAllowed = "move";
            e.dataTransfer.setData("itemAId", element.areaId);
            e.dataTransfer.setData("itemValue", element.value);
        },
        onDrop(e, areaId) {
            console.log(e);
            const itemId = e.dataTransfer.getData("itemAId").split(",");
            const elem = parseInt(e.dataTransfer.getData("itemValue"));
            this.buttons = this.buttons.map((z) => {
                if (
                    arraysEqual(z.areaId, itemId) &&
                    z.value === elem &&
                    z.areaId.includes(areaId)
                ) {
                    z.locateId = areaId;
                    if (areaId === 1) {
                        (z.left = 37.5), (z.top = 37.5);
                    } else if (areaId === 2) {
                        (z.left = 137.5), (z.top = 37.5);
                    } else if (areaId === 3) {
                        (z.left = 37.5), (z.top = 137.5);
                    } else if (areaId === 4) {
                        (z.left = 137.5), (z.top = 137.5);
                    }
                } else {
                    console.log(1);
                }
                return z;
            });
        },
    },
};
</script>

<style>
#app {
    margin-top: 25vh;
}
#block {
    width: 300px;
    height: 300px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    margin: 0 auto;
    overflow: hidden;
}
.circle {
    position: absolute;
    width: 25px;
    height: 25px;
    border-radius: 50%;
    background-color: #0b93b4;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: move;
    transition: cubic-bezier(0.39, 0.575, 0.565, 1);
}

.unselectable {
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -khtml-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}
.selected {
    opacity: 0.6;
}
</style>
