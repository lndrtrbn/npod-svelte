<script lang="ts">
    import { cursorHoverService } from "../../services/cursor-hover.service";
    import { onMount } from 'svelte';
    import { Link } from "svelte-routing";

    let menuRef: HTMLElement;

    onMount(() => {
        // Fetch each item of the menu and mark them for the menu animation
        // that is managed in the cursorService.
        const menuItems: NodeListOf<HTMLElement> = menuRef.querySelectorAll("a");
        menuItems.forEach(item => cursorHoverService.addFocusableElement(item));
    })
</script>

<!--  -->
<!--  -->
<!--  -->
<!--  -->
<!--  -->

<nav bind:this={menuRef} class="menu">
    <Link to="/apod/today">Picture of the Day</Link>
    <Link to="/apod/all-pictures">All pictures</Link>
    <Link to="/about">About</Link>
</nav>

<!--  -->
<!--  -->
<!--  -->
<!--  -->
<!--  -->

<style lang="scss">
    .menu {
        display: flex;
        height: 60px;
        align-items: flex-end;
        justify-content: flex-end;
        padding-right: 40px;

        :global(a) {
            color: var(--color-text-half);
            text-shadow: 0px 0px 5px var(--color-bg);
            text-decoration: none;
            margin-right: 30px;

            &:hover, &[aria-current=page] {
                color: var(--color-primary);
            }
        }
    }
</style>