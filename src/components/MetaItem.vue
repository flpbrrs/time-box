<script lang="ts">
import { IconStar, IconArrowBigRight } from "@tabler/icons-vue"
export default {
    components: {
        IconStar, IconArrowBigRight
    }
}
</script>
<template>
    <div class="meta">
        <span>
            <slot></slot>
        </span>
        <IconStar :size="48" stroke-width="2" />
        <IconArrowBigRight :size="48" stroke-width="2" />
    </div>
</template>
<style scoped>
.meta {
    display: flex;
    align-items: center
}

.meta span {
    flex: 1;
}

.meta * {
    border-bottom: none !important;
}
</style>