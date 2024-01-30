<script setup>
    import { ref } from 'vue';

    const positions = ref({
        clientX: undefined,
        clientY: undefined,
        movementX: 0,
        movementY: 0
    });

    const draggableContainer = ref(null);

    const dragMouseDown = (event) => {
        event.preventDefault();
        positions.value.clientX = event.clientX;
        positions.value.clientY = event.clientY;
        document.onmousemove = elementDrag;
        document.onmouseup = closeDragElement;
    }

    const elementDrag = (event) => {
        event.preventDefault();
        positions.value.movementX = positions.value.clientX - event.clientX;
        positions.value.movementY = positions.value.clientY - event.clientY;
        positions.value.clientX = event.clientX;
        positions.value.clientY = event.clientY;
        draggableContainer.value.style.top = (draggableContainer.value.offsetTop - positions.value.movementY) + 'px';
        draggableContainer.value.style.left = (draggableContainer.value.offsetLeft - positions.value.movementX) + 'px';
    }

    const closeDragElement = () => {
        document.onmouseup = null;
        document.onmousemove = null;
    };
</script>

<template>
    <div ref="draggableContainer" id="draggable-container">
    <div id="draggable-header" @mousedown="dragMouseDown">
        <slot name="header"></slot>
    </div>
        <slot name="main"></slot>
        <slot name="footer"></slot>
    </div>
</template>

<style>
#draggable-container {
    position: absolute;
    z-index: 9;
}
#draggable-header {
    z-index: 10;
}
</style>