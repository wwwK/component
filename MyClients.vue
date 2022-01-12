<template>
  <div class="myclients__content">
    <Title :data="{ text: 'Клиенты' }" />
    <div class="search-block">
      <div class="form-group">
        <img src="~@/assets/img/icons/loop.svg" alt="loop" class="loop" />
        <input type="text" placeholder="Поиск клиента" />
        <button
          type="button"
          :class="{ active: showFilter }"
          @click="toggleFilter"
        ></button>
      </div>
      <transition name="fade">
        <div class="search-block__filter" v-if="showFilter">
          <div class="top-filter">
            <ul>
              <li>
                <label for="all-left_category">
                  <input
                    type="radio"
                    name="left-category"
                    id="all-left_category"
                    checked
                  />
                  <span>все</span>
                </label>
              </li>
              <li>
                <label for="left-category1">
                  <input
                    type="radio"
                    name="left-category"
                    id="left-category1"
                  />
                  <span>фармацевты</span>
                </label>
              </li>
              <li>
                <label for="left-category2">
                  <input
                    type="radio"
                    name="left-category"
                    id="left-category2"
                  />
                  <span>врачи</span>
                </label>
              </li>
            </ul>

            <ul>
              <li>
                <label for="all-right_category">
                  <input
                    type="radio"
                    name="right-category"
                    id="all-right_category"
                    checked
                  />
                  <span>все</span>
                </label>
              </li>
              <li>
                <label for="right-category1">
                  <input
                    type="radio"
                    name="right-category"
                    id="right-category1"
                  />
                  <span>с согласием</span>
                </label>
              </li>
              <li>
                <label for="right-category2">
                  <input
                    type="radio"
                    name="right-category"
                    id="right-category2"
                  />
                  <span>без согласия</span>
                </label>
              </li>
            </ul>
          </div>
          <div class="bottom-filter">
            <ul class="choise">
              <li v-for="(speciality, index) in specialities" :key="index">
                {{ speciality }}
                <img
                  @click="specialitySelectionHandler(speciality)"
                  src="~@/assets/img/icons/remove-filter.svg"
                  alt="rempve"
                />
              </li>
            </ul>

            <div class="dropdown-select" :class="{ active: showDropdown }">
              <button class="title" @click="toggleDropdown">
                Добавить специальность
                <img src="~@/assets/img/icons/select-arrow.svg" alt="arrow" />
              </button>
              <ul>
                <li
                  v-for="(speciality, index) in specialityOptions"
                  :key="index"
                  :class="{ active: specialities.indexOf(speciality) !== -1 }"
                  @mousedown="specialitySelectionHandler(speciality)"
                >
                  {{ speciality }}
                </li>
              </ul>
            </div>
          </div>
        </div>
      </transition>
    </div>

    <div class="clients-block">
      <div
        class="clients-block__item"
        v-for="(clientInfo, i) of clientsInfo"
        :key="i"
        :class="{ 'status-red': !clientInfo.status }"
      >
        <div class="head">
          <div class="head__top-info" @click="showAccordin">
            <h4>{{ clientInfo.name }}</h4>
            <div class="status red" v-if="!clientInfo.status">
              не подтверждён
            </div>
            <button class="toggle-btn"></button>
          </div>
          <div class="head__bottom-info">
            <div class="info">
              <div class="kind">{{ clientInfo.kind }}</div>
              <div class="institution">
                {{ clientInfo.institution }}
              </div>
            </div>
            <div class="phone">{{ clientInfo.phone }}</div>
          </div>
        </div>
        <div class="body">
          <div class="body__block">
            <div class="body__block-item copy-link">
              <span>Телефон</span>
              <p>
                <a :href="`tel:${clientInfo.phone}`">{{ clientInfo.phone }}</a>
                <button class="copy" @click="doCopy">
                  <img src="~@/assets/img/icons/copy.svg" alt="icon" />
                </button>
              </p>
            </div>
            <div class="body__block-item">
              <span>Email</span>
              <p><a href="mailto:galina@gmail.com">galina@gmail.com</a></p>
            </div>
            <div class="body__block-item">
              <span>Учреждение</span>
              <p>{{ clientInfo.institution }}</p>
            </div>
            <div class="body__block-item">
              <span>Адрес</span>
              <p>{{ clientInfo.address }}</p>
            </div>
            <div class="body__block-item agreement">
              <span>Согласие</span>
              <p v-if="clientInfo.agreement" style="color: #00a03b">
                <img src="~@/assets/img/icons/client-false.svg" alt="icon" />
                имеется
              </p>
              <p v-else style="color: #aa198d">
                <img
                  src="~@/assets/img/icons/close.svg"
                  alt="icon"
                  class="agreement-false-img"
                />
                отсутствует
              </p>
            </div>
            <div class="body__block-item copy-link">
              <span>Уникальная ссылка</span>
              <p v-if="clientInfo.uniqueLink">
                <span class="copied-link">{{ clientInfo.uniqueLink }}</span>
                <button class="copy" @click="doCopy">
                  <img src="~@/assets/img/icons/copy.svg" alt="icon" />
                </button>
              </p>
              <p v-else><button class="generate">Сгенерировать</button></p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="bottom-navigation">
      <p class="total">Всего: 1234</p>
      <ul class="pagination">
        <li class="active">1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
        <li>...</li>
        <li>15</li>
      </ul>
    </div>
  </div>
</template>

<script>
import Title from "@/components/UI/Title";

export default {
  name: "MyClients",
  components: {
    Title,
  },
  data() {
    return {
      showDropdown: false,
      showFilter: false,
      specialities: [],
      specialityOptions: ["Акушерство", "Гинекология", "Урология"],
      clientsInfo: [
        {
          status: true,
          name: "Арсен Венгер",
          kind: "Терапия",
          phone: "87052331664",
          email: "ork@gmail.com",
          institution: "Поликлиника №11 Больницы областной хирургии",
          address: "Казахстан, Шымкент, мкр Север, д 1656, кв 2021",
          agreement: true,
          uniqueLink: "https://jira.rocketfirm.com/brows/brows/brows/brows",
        },
        {
          status: false,
          name: "Абрамова Галина Анатольевна",
          kind: "Терапия",
          phone: "870754656565",
          email: "sdak@gmail.com",
          institution: "Поликлиника №1 Больницы областной хирургии",
          address: "Казахстан, Алматы, мкр Север, д 1656, кв 2021",
          agreement: false,
          uniqueLink: null,
        },
      ],
    };
  },
  methods: {
    specialitySelectionHandler(speciality) {
      let index = this.specialities.indexOf(speciality);
      if (index === -1) {
        this.specialities.push(speciality);
        return;
      }
      this.specialities.splice(index, 1);
    },
    toggleDropdown() {
      this.showDropdown = !this.showDropdown;
    },
    toggleFilter() {
      this.showFilter = !this.showFilter;
    },
    showAccordin(event) {
      event.target.parentElement.classList.toggle("active");
    },
    doCopy(event) {
      this.$copyText(event.target.parentElement.textContent).then(
        function (e) {
          alert("Copied");
          console.log(e);
        },
        function (e) {
          alert("Can not copy");
          console.log(e);
        }
      );
    },
  },
};
</script>