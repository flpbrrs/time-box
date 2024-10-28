<script lang="ts">
export default {
    props: {
        emptyMessage: { type: String, default: "Lista vazia" }
    }
}
</script>
<template>
    <div class="squared-list">
        <slot>
            <p class="empty">{{ emptyMessage }}</p>
        </slot>
    </div>
</template>
<style scoped>
.squared-list {
    border: 1px solid #2226;
}

.squared-list *:nth-child(n) {
    padding: 12px 8px;
    border-bottom: 1px solid #2226;
    font-size: 1.2rem;
}

.squared-list *:last-child {
    border-bottom: 0px;
}

.empty {
    text-align: center;
    color: #2228;
}
</style>