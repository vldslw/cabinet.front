<template>
  <main class="unload">
    <section class="unload__left">
      <div class="block unload__desc">
        <h2 class="unload__desc-title">Выгрузка</h2>
        <h3 class="unload__desc-subtitle"><span class="text-bold">Выполняет работу:</span></h3>
        <ul class="unload__desc-list">
          <li class="unload__desc-item"> - Собирает фотографии из заказов пользователей.</li>
          <li class="unload__desc-item"> - Выгружает по папкам.</li>
        </ul>
      </div>
      <div v-for="card in cards" :key="card.id" class="block unload__card">
        <p class="unload__card-text">Задача выполнена: <span class="text-bold">{{ card.date }}</span></p>
        <p class="unload__card-text">Статус задачи: <span class="text-bold">{{ card.status_text }}</span> </p>
        <p class="unload__card-text">ID выгрузки: <span class="text-bold">{{ card.id }}</span></p>
        <p class="unload__card-text">{{ card.event }}</p>
        <p class="unload__card-text">Размер выгрузки: <span class="text-bold">{{ card.size }}</span></p>
      </div>
    </section>
    <section class="unload__right">
      <div class="notice unload__info" data-color="light-purple">
        <p>Для того, чтобы посмотреть информацию о <span class="text-bold">выгрузке</span>, а также ее скачать, нажмите на требуемую выгрузку в столбце слева</p>
      </div>
    </section>
  </main>
</template>

<script setup>
  const { data } = await useFetch(`https://dev-cabinet.seenday.com/e.scripts?page`, {
    query: { page: 'pages:unload', event: 'get' }
  });
  const jsonData = await JSON.parse(data.value);
  const cards = toRaw(jsonData.response.data);
</script>

<style lang="scss" scoped>

</style>