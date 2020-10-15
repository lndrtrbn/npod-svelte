<script lang="ts">
    import paper from "paper";
    import { cursorHoverService } from "../../services/cursor-hover.service";
    import type { CursorCircleConf } from "./cursor-circle-conf";
    import type { CursorPosition } from "../../interfaces/cursor-position";
    import { onMount } from 'svelte';

    let smallCursorRef: HTMLDivElement;

    let position: CursorPosition = { x: -100, y: -100 };
    let lastPosition: CursorPosition = { x: -100, y: -100 };
    let stuckPosition: CursorPosition = { x: -100, y: -100 };

    let circleConf: CursorCircleConf = {
        strokeColor: new paper.Color(1, 1, 1, 0.2),
        strokeColorHover: new paper.Color(0.176, 0.855, 0.722, 0.8),
        strokeWidth: 1,
        segments: 8,
        radius: 15
    };

    // Determines if the cursor should stick to an element.
    let isNoisy: boolean = false;

    // Paper object containing our circle.
    let polygon: paper.Path.RegularPolygon;

    onMount(() => {
        // Update position.
        document.addEventListener('mousemove', (event) => {
            position = { x: event.clientX, y: event.clientY };
        });

        // Listen for stuck position changes.
        cursorHoverService.stuckPosition.subscribe(p => stuckPosition = p);

        new paper.Project("cursorCanvas");
        // The base shape for the noisy circle.
        polygon = buildCirclePolygon();
        // The draw loop of Paper.js.
        // (60fps with requestAnimationFrame under the hood).
        paper.view.onFrame = () => {
            renderCircleCursor();
            renderSmallCursor();
        };
    });

    /**
     * Computes the position of the circle of the cursor.
     */
    function renderCircleCursor(): void {
        // function for linear interpolation of values
        const lerp = (a: number, b: number, n: number) => (1 - n) * a + n * b;
        // Using linear interpolation, the circle will move 0.2 (20%)
        // of the distance between its current position and the mouse
        // coordinates per Frame.
        if (!stuckPosition) {
            lastPosition.x = lerp(lastPosition.x, position.x, 0.2);
            lastPosition.y = lerp(lastPosition.y, position.y, 0.2);
            polygon.strokeColor = circleConf.strokeColor;
            if (isNoisy) {
                polygon.scale(0.5, 200);
                isNoisy = false;
            }
        } else {
            lastPosition.x = lerp(lastPosition.x, stuckPosition.x, 0.2);
            lastPosition.y = lerp(lastPosition.y, stuckPosition.y, 0.2);
            polygon.strokeColor = circleConf.strokeColorHover;
            if (!isNoisy) {
                polygon.scale(2, 0.005);
                isNoisy = true;
            }
        }
        polygon.position = new paper.Point(lastPosition.x, lastPosition.y);
    }

    /**
     * Computes the position of the small cursor.
     */
    function renderSmallCursor(): void {
        const newPosition = `translate(${position.x}px, ${position.y}px)`;
        smallCursorRef.style.transform = newPosition;
    }

    /**
     * Builds the circle polygon that follow the cursor.
     * 
     * @returns The paper object Polygon.
     */
    function buildCirclePolygon(): paper.Path.RegularPolygon {
        const polygon = new paper.Path.RegularPolygon(
            new paper.Point(0, 0),
            circleConf.segments,
            circleConf.radius
        );
        polygon.strokeColor = circleConf.strokeColor;
        polygon.strokeWidth = circleConf.strokeWidth;
        polygon.smooth();
        return polygon;
    }
</script>

<!--  -->
<!--  -->
<!--  -->
<!--  -->
<!--  -->

<div bind:this={smallCursorRef} class="cursor cursor--small"></div>
<canvas id="cursorCanvas" class="cursor cursor--canvas" resize></canvas>

<!--  -->
<!--  -->
<!--  -->
<!--  -->
<!--  -->

<style lang="scss">
    .cursor {
        position: fixed;
        left: 0;
        top: 0;
        pointer-events: none;
    }
    .cursor--small {
        width: 5px;
        height: 5px;
        left: -2.5px;
        top: -2.5px;
        border-radius: 50%;
        z-index: 11000;
        background: var(--color-text);
    }
    .cursor--canvas {
        width: 100vw;
        height: 100vh;
        z-index: 12000;
    }
</style>