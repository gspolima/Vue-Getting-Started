<template>
  <div class="content-container">
    <div class="section content-title-group">
      <h2 class="title">Heroes</h2>
    </div>
    <div class="notification is-secondary">Random Fact: {{ randomFact }}</div>
    <div class="columns">
      <div class="column is-3">
        <div class="card" v-show="heroes.length">
          <header class="card-header">
            <p class="card-header-title">heroes list</p>
          </header>
          <ul class="list is-hoverable">
            <li v-for="hero in heroes" :key="hero.id">
              <a
                class="list-item"
                @click="selectHero(hero)"
                :class="{ 'is-active': selectedHero === hero }"
              >
                <span>{{ hero.firstName }}</span>
              </a>
            </li>
          </ul>
        </div>
        <div class="notification is-info" v-show="message">{{ message }}</div>
      </div>

      <div class="column is-4" v-if="selectedHero">
        <div class="card">
          <header class="card-header">
            <p class="card-header-title">{{ fullName }}</p>
          </header>
          <div class="card-content">
            <div class="content">
              <div class="field">
                <label class="label" for="id">id</label>
                <label class="input" id="id" readonly>{{
                  selectedHero.id
                }}</label>
              </div>
              <div class="field">
                <label class="label" for="firstName">first name</label>
                <input
                  class="input"
                  id="firstName"
                  v-model="selectedHero.firstName"
                />
              </div>
              <div class="field">
                <label class="label" for="lastName">last name</label>
                <input
                  class="input"
                  id="lastName"
                  v-model="selectedHero.lastName"
                />
              </div>
              <div class="field">
                <label class="label" for="description">description</label>
                <input
                  class="input"
                  id="description"
                  v-model="selectedHero.description"
                  :maxlength="descriptionCharLimit"
                />
                <p>{{ charactersUsed }}/{{ descriptionCharLimit }}</p>
              </div>
            </div>
          </div>
          <footer class="card-footer">
            <button
              class="link card-footer-item cancel-button"
              @click="cancelHero()"
            >
              <i class="fas fa-undo"></i>
              <span>Cancel</span>
            </button>
            <button class="link card-footer-item" @click="saveHero()">
              <i class="fas fa-save"></i>
              <span>Save</span>
            </button>
          </footer>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const ourHeroes = [
  {
    id: 10,
    firstName: 'Ella',
    lastName: 'Papa',
    description: 'fashionista',
  },
  {
    id: 20,
    firstName: 'Madelyn',
    lastName: 'Papa',
    description: 'the cat whisperer',
  },
  {
    id: 30,
    firstName: 'Haley',
    lastName: 'Papa',
    description: 'pen wielder',
  },
  {
    id: 40,
    firstName: 'Landon',
    lastName: 'Papa',
    description: 'arc trooper',
  },
];

export default {
  name: 'Heroes',
  data() {
    return {
      heroes: [],
      selectedHero: undefined,
      message: '',
      descriptionCharLimit: 50,
      randomFact: '',
    };
  },
  computed: {
    fullName: {
      get() {
        if (this.selectedHero) {
          let name = this.selectedHero.firstName;
          name += this.selectedHero.lastName
            ? ` ${this.selectedHero.lastName}`
            : '';
          return name;
        }
        return '';
      },
      set(value) {
        let names = value.split(' ');
        this.selectedHero.firstName = names[0];
        this.selectedHero.lastName =
          names.length === 1 ? '' : names[names.length - 1];
      },
    },
    reversedDescription: function() {
      const desc = this.selectedHero.description;
      return desc.reverse();
    },
    charactersUsed: function() {
      return this.selectedHero.description.length;
    },
  },
  created() {
    this.loadHeroes();
    this.loadRandomFact();
  },
  methods: {
    handleTheCapes(newValue) {
      const value = parseInt(newValue, 10);
      switch (value) {
        case 0:
          this.capeMessage = 'Where is my cape?';
          break;
        case 1:
          this.capeMessage = 'One is all I need';
          break;
        case 2:
          this.capeMessage = 'Alway have a spare';
          break;
        default:
          this.capeMessage = 'You can never have enough capes';
          break;
      }
    },
    cancelHero() {
      this.selectedHero = undefined;
      this.message = '';
    },
    saveHero() {
      // This only updates when you click the save button
      this.message = JSON.stringify(this.selectedHero, null, '\n ');
    },
    selectHero(hero) {
      this.selectedHero = hero;
    },
    async getHeroes() {
      return new Promise(resolve => {
        setTimeout(() => resolve(ourHeroes), 1500);
      });
    },
    async loadHeroes() {
      this.heroes = [];
      this.message = 'Loading heroes. Please be patient.';
      this.heroes = await this.getHeroes();
      this.message = '';
    },
    getRandomFact() {
      let request = new XMLHttpRequest();
      request.open(
        'GET',
        'http://uselessfacts.jsph.pl/random.txt?language=en',
        false
      );
      request.send(null);
      return request.responseText;
    },
    loadRandomFact() {
      let text = this.getRandomFact();
      let index = text.indexOf('Source');
      let trimmedText = text.slice(0, index);
      this.randomFact = trimmedText;
    },
  },
};
</script>
