<script lang="ts">
import Logo from './components/Logo.vue';
import MetaItem from './components/MetaItem.vue';
import SquaredList from './components/SquaredList.vue';

export interface IMetaItem {
  label: string
  inTop: boolean
  inBacklog: boolean
}

export default {
  data() {
    return {
      currentDate: new Date().toISOString().slice(0, 10),
      currentMeta: '' as string,
      horasDoDia: Array.from({ length: 19 }, (_, index) => String(index + 5).padStart(2, '0')),
      metas: [] as IMetaItem[],
    }
  },
  methods: {
    handleMetaAdd() {
      if (this.currentMeta === '') return;
      if (this.metas.filter(m => m.label === this.currentMeta).length > 0) {
        this.currentMeta = ''
        return;
      };
      this.metas.push({
        label: this.currentMeta,
        inTop: false,
        inBacklog: true,
      });
      this.currentMeta = ''
    },
    handleMetaDailyTop(newTopMeta: IMetaItem) {
      if (newTopMeta.inTop) {
        newTopMeta.inTop = false
        return;
      }

      if (this.metas.filter(m => m.inTop).length >= 3) return;
      newTopMeta.inTop = true
    }
  },
  components: { Logo, SquaredList, MetaItem }
}
</script>

<template>
  <main>
    <div class="container">
      <section class="form">
        <div class="logo-wrapper">
          <Logo />
        </div>
        <div class="inputform">
          <label for="data">Data:</label>
          <input type="date" v-model="currentDate" disabled>
        </div>
        <h3>Top metas diárias</h3>
        <SquaredList :empty-message="'Escolha as metas prioritárias do dia'">
          <MetaItem v-for="meta in metas.filter(m => m.inTop)" :key="`meta-${meta}`" :itemData="meta"
            @toogle-top-meta="handleMetaDailyTop" />
        </SquaredList>
        <h3>Metas do dia</h3>
        <div class="inputform">
          <input type="text" v-model="currentMeta" name="metas" placeholder="Ex.: Passear com o cachorro...">
          <button type="button" @click="handleMetaAdd" :disabled="currentMeta.length <= 0">Adicionar</button>
        </div>
        <SquaredList :empty-message="'Definas suas metas do dia'">
          <MetaItem v-for="meta in metas.filter(m => !m.inTop)" :key="`meta-${meta}`" :itemData="meta"
            @toogle-top-meta="handleMetaDailyTop" />
        </SquaredList>
      </section>
      <section class="timebox">
        <h3>Horários</h3>
        <div class="calendar-wrapper">
          <div v-for="hora in horasDoDia" :key="hora">
            <span class="number">
              {{ hora }}
            </span>
          </div>
          <div class="dragger-wrapper" />
        </div>
      </section>
    </div>
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 24px;

  min-height: 100vh;
}

h3 {
  font-weight: bold;
  font-size: 1.2rem;
}

button {
  background-color: transparent;
  padding: 8px 12px;
  cursor: pointer;
  outline: none;

  transition: all 50ms ease-in-out;
}

button:hover {
  transform: scale(1.02)
}

input::placeholder {
  font-weight: lighter;
  opacity: 0.5;
  color: #333c;
  font-size: 1rem;
}

.container {
  display: flex;
  gap: 24px;
}

.form,
.timebox {
  display: flex;
  flex-direction: column;
  gap: 12px;
  width: 400px;
}

.inputform {
  display: flex;
  justify-content: center;
  gap: 12px;
  font-size: 1.5rem;

  padding: 8px 0;
  border-bottom: 1px solid #222;
}

.inputform label {
  font-weight: bold;
}

.inputform input {
  width: 100%;
  text-align: center;
  font-size: 1.5rem;

  border: 0;
  outline: none;
  background-color: transparent;
}

.logo-wrapper {
  margin-bottom: 32px;
}

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

.calendar-wrapper,
.lista-metas {
  border: 1px solid #2226;
}

.lista-metas {
  padding: 8px;
  font-size: 1.2rem;
}

.top-goals-wrapper p:nth-child(n),
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

.top-goals-wrapper p:last-child,
.calendar-wrapper div:last-child {
  border-bottom: 0px;
}
</style>
