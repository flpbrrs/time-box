<script lang="ts">
import { GridLayout, GridItem } from 'grid-layout-plus'
import type { PropType } from 'vue';

export default {
    data() {
        return {
            horasDoDia: Array.from({ length: 19 }, (_, index) => String(index + 5).padStart(2, '0')),
            layout: [] as any,
            refId: 0
        }
    },
    props: {
        data: { type: Array as PropType<any[]>, required: true }
    },
    methods: {
        addItemIntoTimebox(item: any) {
            let novoItem = {
                i: String(this.refId++), x: 0, y: 0, w: 1, h: 1, content: item.label
            }
            this.layout.push(novoItem)
        },
        removeItemFromTimebox(item: any) {
            let items = this.layout.filter((i: any) => i.content != item.label)
            this.layout = [...items]
        }
    },
    updated() {
        const itensInTimebox = this.$props.data.filter(m => !m.inBacklog)

        const novosItems = itensInTimebox.filter(
            novoItem => !this.$data.layout.some(
                (originalItem: any) => originalItem.content === novoItem.label
            )
        )

        if (novosItems.length >= 1) {
            const items = novosItems.map((m) => ({
                i: String(itensInTimebox.length), x: 0, y: 0, w: 1, h: 1, content: m.label
            }))
            this.layout = [...this.layout, ...items]
        }
    },
    components: {
        GridLayout,
        GridItem
    }
}
</script>
<template>
    <section class="app-section timebox-container">
        <h3>Horários</h3>
        <div class="calendar-wrapper list-wrapper-bordered ">
            <div v-for="hora in horasDoDia" :key="hora">
                <span class="number">
                    {{ hora }}
                </span>
            </div>
        </div>
        <div class="dragger-wrapper">
            <GridLayout v-model:layout="layout" :col-num="1" :row-height="56" is-draggable is-resizable :margin="[8, 0]"
                use-css-transforms style="height: 100%;" prevent-collision :responsive="false"
                :vertical-compact="false">
                <template #item="{ item }">
                    <span class="grid-item">
                        {{ item.content }}
                    </span>
                </template>
            </GridLayout>
        </div>
    </section>
</template>
<style scoped>
.timebox-container {
    position: relative;
}

.dragger-wrapper {
    position: absolute;
    top: 44px;
    bottom: 0;
    left: 52px;
    right: 0;
}

.calendar-wrapper div:nth-child(n) {
    padding: 12px 8px;
    border-bottom: 1px solid #2226;
    font-size: 1.2rem;
}

.calendar-wrapper .number {
    font-weight: bold;
    text-align: center;
    margin: 0 8px;
    color: #2226;
    user-select: none
}

.calendar-wrapper div:last-child {
    border-bottom: 0px;
}

.grid-item {
    border: 1px solid #222;
    background-color: #f7f7f7;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    user-select: none
}
</style>