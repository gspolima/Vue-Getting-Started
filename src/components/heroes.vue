<template>
  <div class="content-container">
    <div class="section content-title-group">
      <h2 class="title">Heroes</h2>
    </div>
    <div class="columns">
      <div class="column is-3">
        <header class="card-header">
          <p class="card-header-title">heroes list</p>
        </header>
        <ul class="list is-hoverable">
          <li v-for="hero in heroes" :key="hero.id">
            <a
              @click="displayDetails(hero)"
              class="list-item"
              :class="{ 'is-active': selectedHero === hero }"
            >
              <span>{{ hero.firstName }}</span>
            </a>
          </li>
        </ul>
      </div>
    </div>
    <div v-if="typeof selectedHero !== 'undefined'" class="columns">
      <div class="column is-3">
        <header class="card-header">
          <p class="card-header-title">{{ selectedHero.firstName }}</p>
        </header>
        <div class="card-content">
          <div class="content">
            <div class="field">
              <label class="label" for="id">id</label>
              <label class="input" id="id" readonly>
                {{ selectedHero.id }}
              </label>
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
              <input
                class="is-primary"
                @click="showState"
                v-model="hideLastNameAndDescription"
                type="checkbox"
                name="hideLastNameAndDescription"
              />
              <label for="hid eLastNameAndDescription">
                Hide last name and description
              </label>
            </div>
            <div v-show="hideLastNameAndDescription === false">
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
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Heroes',
  data() {
    return {
      hideLastNameAndDescription: false,
      selectedHero: undefined, // preferable over an empty object
      heroes: [
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
        {
          id: 50,
          firstName: 'Gustavo',
          lastName: 'Sampaio',
          description: 'dragon slayer',
        },
      ],
    };
  },
  methods: {
    displayDetails(hero) {
      console.log(hero);
      this.selectedHero = hero;
    },
    showState() {
      console.log(
        `The checkbox for showing the last two fields was toggled to 
        ${!this.hideLastNameAndDescription}`
      );
    },
  },
};
</script>
