<script lang="ts">
export default {
    data() {
        return {
            horasDoDia: Array.from({ length: 19 }, (_, index) => String(index + 5).padStart(2, '0')),
        }
    }
}
</script>
<template>
    <section class="app-section">
        <h3>Horários</h3>
        <div class="calendar-wrapper list-wrapper-bordered ">
            <div v-for="hora in horasDoDia" :key="hora">
                <span class="number">
                    {{ hora }}
                </span>
            </div>
            <div class="dragger-wrapper" />
        </div>
    </section>
</template>
<style scoped>
.calendar-wrapper {
    position: relative;
}

.dragger-wrapper {
    position: absolute;
    top: 0;
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
}

.calendar-wrapper div:last-child {
    border-bottom: 0px;
}
</style>