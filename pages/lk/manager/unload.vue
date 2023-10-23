<template>
  <main class="unload">
    <section class="block unload__desc">
      <h2 class="unload__desc-title">Выгрузка</h2>
      <h3 class="unload__desc-subtitle"><span class="text-bold">Выполняет работу:</span></h3>
      <ul class="unload__desc-list">
        <li class="unload__desc-item"> - Собирает фотографии из заказов пользователей.</li>
        <li class="unload__desc-item"> - Выгружает по папкам.</li>
      </ul>
    </section>
    <section class="unload__cards">
      <div 
        v-for="card in cards" 
        :key="card.id" class="block unload__card" :card-status="card.status"
        @click="fetchLink(card.id)">
        <p class="unload__card-text">Задача выполнена: <span class="text-bold">{{ card.date }}</span></p>
        <p class="unload__card-text">Статус задачи: <span class="text-bold">{{ card.status_text }}</span> </p>
        <p class="unload__card-text">ID выгрузки: <span class="text-bold">{{ card.id }}</span></p>
        <p class="unload__card-text">{{ card.event }}</p>
        <p class="unload__card-text">Размер выгрузки: <span class="text-bold">{{ card.size }}</span></p>
      </div>
    </section>   
    <section class="unload__sticky">
      <div v-if="showInfo === true" class="notice unload__info" data-color="light-purple">
        <p>Для того, чтобы посмотреть информацию о <span class="text-bold">выгрузке</span>, а также ее скачать, нажмите на требуемую выгрузку в столбце слева</p>
      </div>
      <div v-else class="block unload__download">
        <h2 class="unload__download-title"><span class="text-bold">Ссылка для скачивания архива Выгрузки (.zip):</span></h2>
        <a class="link unload__download-link" :href="link">{{ link }}</a>
        <button class="span-link unload__download-btn" @click="copyToClipboard(link)">cкопировать ссылку</button>
      </div>
    </section>
  </main>
</template>

<script>
  export default {
    async setup() {
    const { data } = await useFetch(`https://dev-cabinet.seenday.com/e.scripts?page`, {
      query: { page: 'pages:unload', event: 'get' }
    });
    const jsonData = JSON.parse(data.value);
    const cards = toRaw(jsonData.response.data);
    return { cards };
    },
    data () {
      return {
        showInfo: true,
        link: '',
      }
    },
    methods: {
      async fetchLink(id) {
        const { data } = await useFetch(`https://dev-cabinet.seenday.com/e.scripts?page`, {
          query: { page: 'pages:unload', event: 'get', unload_id: id }
        });
        const jsonData = JSON.parse(data.value);
        const cardLink = toRaw(jsonData.response.data[0].download_link);
        this.link = cardLink;
        this.showInfo = false;
      },
      copyToClipboard (link) {
        navigator.clipboard.writeText(link)
      },
    }
  }
</script>

<style lang="scss" scoped>

</style>