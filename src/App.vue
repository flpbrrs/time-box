<script lang="ts">
import Logo from './components/Logo.vue';
import MetaItem from './components/MetaItem.vue';
import SquaredList from './components/SquaredList.vue';
import TimeBox from './components/TimeBox.vue';

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
    },
    handleDeleteMeta(meta: IMetaItem) {
      this.metas = this.metas.filter(m => m.label !== meta.label)
    }
  },
  components: { Logo, SquaredList, MetaItem, TimeBox }
}
</script>

<template>
  <main>
    <div class="container">
      <section class="app-section">
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
            @toogle-top-meta="handleMetaDailyTop" @delete-meta="handleDeleteMeta" />
        </SquaredList>
        <h3>Metas do dia</h3>
        <div class="inputform">
          <input type="text" v-model="currentMeta" name="metas" placeholder="Ex.: Passear com o cachorro...">
          <button type="button" @click="handleMetaAdd" :disabled="currentMeta.length <= 0">Adicionar</button>
        </div>
        <SquaredList :empty-message="'Definas suas metas do dia'">
          <MetaItem v-for="meta in metas.filter(m => !m.inTop)" :key="`meta-${meta}`" :itemData="meta"
            @toogle-top-meta="handleMetaDailyTop" @delete-meta="handleDeleteMeta" />
        </SquaredList>
      </section>
      <TimeBox />
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
</style>
