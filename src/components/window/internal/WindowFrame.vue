<script setup>
    import { ref } from 'vue';

    const positions = ref({
        clientX: undefined,
        clientY: undefined,
        movementX: 0,
        movementY: 0
    });

    const windowContainer = ref(null);

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
        windowContainer.value.style.top = (windowContainer.value.offsetTop - positions.value.movementY) + 'px';
        windowContainer.value.style.left = (windowContainer.value.offsetLeft - positions.value.movementX) + 'px';
    }

    const closeDragElement = () => {
        document.onmouseup = null;
        document.onmousemove = null;
    };
</script>

<template>
    <div ref="windowContainer" class="absolute z-10 bg-white rounded-t-lg border-1 border-black border-opacity-100 shadow" id="window-frame-x" aria-labelledby="modal-title" role="dialog" aria-modal="true">
        <div id="draggable-header" class="bg-gray-500 rounded-t-lg" @mousedown="dragMouseDown">
            <div class="px-5 pt-1 pb-1 flex">
                <div class="window-title flex-grow">
                    <slot name="title"></slot>
                </div>
                <div class="window-actions flex-none w-16">
                </div>
            </div>
        </div>
        <div class="window-content px-5 pb-10 pt-10">
            <slot name="main"></slot>
        </div>
        
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