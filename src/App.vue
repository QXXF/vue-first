<script async setup lang="ts">
  import { onMounted, ref, type Ref } from 'vue';
  import { wrapGrid } from 'animate-css-grid'

  import { ep } from './constants';
  import Figure from '@/Figure.vue'
  import RandIcon from '@/assets/RandIcon.vue'

  let list: Ref<any[]> = ref([])

  onMounted( async () => {
    list.value = await fetch(ep).then((r) => r.json())
    const grid = document.querySelector("main.container") as HTMLElement;
    wrapGrid(grid, { 
      duration: 600,
      easing: 'backInOut' 
    });
  })

  const sortByDate = () => {
    list.value = list.value.sort( (a, b) => {
        // Converti le date in oggetti Date
        const dateA = new Date(a.date);
        const dateB = new Date(b.date);
        // Confronta le date e ritorna -1, 0 o 1
        return dateA > dateB ? 1 : dateA < dateB ? -1 : 0
    })
  }

  const sortRandom = () => {
    list.value = list.value.sort( () => Math.random() - 0.5)
  }

</script>

<template>
  <header class="container">
    <h1>Primo esercizio</h1>
    <div class="actions">
      <button @click=sortByDate >Ordina per data</button>
      <button class="secondary small-btn" @click=sortRandom >
        <RandIcon></RandIcon>
      </button>
    </div>
  </header>

  <main class="container">
      <Figure v-for="item of list" :data="item" :key="item.id" ></Figure>
  </main>
</template>

<style scoped lang="scss">
  main.container {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    grid-gap: 16px;
    margin: 32px auto;
    @media (min-width: 768px) {
      .container {
        grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
      }
    }
  }

  header {
    display: flex;
    align-items: baseline;
    flex-direction: column;
    justify-content: space-between;

    margin-top: 32px;
    gap: 16px;

    button, h1 {
      margin: 0;
      white-space: nowrap;
    }
    .actions {
      width: 100%;
      display: flex;
      gap: 16px;
      .small-btn {
        width: 56px;
      }
    }

    @media (min-width: 768px) {
      flex-direction: row;
      .actions {
        width: auto;
      }
    }
  }
</style>