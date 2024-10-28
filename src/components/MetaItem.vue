<script lang="ts">
import { IconStar, IconStarFilled, IconArrowBigRight } from "@tabler/icons-vue"
import type { IMetaItem } from "@/App.vue";
import type { PropType } from "vue";
export default {
    components: {
        IconStar, IconStarFilled, IconArrowBigRight
    },
    props: {
        itemData: { type: Object as PropType<IMetaItem>, required: true }
    },
    methods: {
        handleFavClick() {
            this.$emit('toogle-top-meta', this.$props.itemData)
        }
    },
    emits: ['toogle-top-meta']
}
</script>
<template>
    <div class="meta">
        <span>{{ itemData.label }}</span>
        <IconStarFilled class="item-btn" v-if="itemData.inTop" :size="48" stroke-width="2" @click="handleFavClick" />
        <IconStar class="item-btn" v-else :size="48" stroke-width="2" @click="handleFavClick" />
        <IconArrowBigRight class="item-btn" :size="48" stroke-width="2" v-if="itemData.inBacklog" />
    </div>
</template>
<style scoped>
.meta {
    display: flex;
    align-items: center
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