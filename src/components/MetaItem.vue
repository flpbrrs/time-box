<script lang="ts">
import { IconStar, IconStarFilled, IconArrowBigRight, IconArrowBigLeft, IconTrash } from "@tabler/icons-vue"
import type { IMetaItem } from "@/App.vue";
import type { PropType } from "vue";
export default {
    components: {
        IconStar, IconStarFilled, IconArrowBigRight, IconArrowBigLeft, IconTrash
    },
    props: {
        itemData: { type: Object as PropType<IMetaItem>, required: true }
    },
    methods: {
        handleFavClick() {
            this.$emit('toogle-top-meta', this.$props.itemData)
        },
        handleDelClick() {
            this.$emit('delete-meta', this.$props.itemData)
        }
    },
    emits: ['toogle-top-meta', 'delete-meta']
}
</script>
<template>
    <div class="meta">
        <span>{{ itemData.label }}</span>
        <IconStarFilled class="item-btn" v-if="itemData.inTop" :size="48" stroke-width="2" @click="handleFavClick" />
        <IconStar class="item-btn" v-else :size="48" stroke-width="2" @click="handleFavClick" />
        <IconTrash class="item-btn" :size="48" stroke-width="2" @click="handleDelClick" />
        <IconArrowBigRight class="item-btn" :size="48" stroke-width="2" v-if="itemData.inBacklog" />
        <IconArrowBigLeft class="item-btn" :size="48" stroke-width="2" v-else />
    </div>
</template>
<style scoped>
.meta {
    display: flex;
    align-items: center;
    gap: 0
}

.meta span {
    flex: 1;
    user-select: none;
}

.meta * {
    border-bottom: none !important;
}

.item-btn {
    cursor: pointer;
}
</style>