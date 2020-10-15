<main class="host">
    <div id="small-cursor" class="cursor cursor--small"></div>
    <canvas id="cursorCanvas" class="cursor cursor--canvas" resize></canvas>
</main>

<style lang="scss">
    .host {
        display: block;
    }
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