<template>
  <div>
    <div>
      <header>
        <h1>MMR Tracker</h1>
      </header>
      <main>
        <div class="search">
          <div class="form-group">
            <label for="region">Region : </label>
            <select v-model="region">
              <option
                :key="index"
                v-for="(reg, index) in regions"
                v-bind:value="reg.value"
              >
                {{ reg.text }}
              </option>
            </select>
          </div>
          <div class="form-group">
            <label for="summoner">Summoner's name : </label>
            <input
              v-on:keyup.enter="fetchData"
              type="summoner"
              id="summoner"
              v-model="summonerName"
            />
          </div>
          <div class="form-group">
            <button
              class="btn btn-primary"
              type="submit"
              v-on:click="fetchData"
            >
              Search
            </button>
          </div>
        </div>
        <br />
      </main>
      <section v-if="errorMessage != ''">
        <div style="text-align:center;">
          <h2>{{ errorMessage }}</h2>
        </div>
      </section>
      <section v-if="loading">
        <div style="text-align:center;">
          <h2>loading data...</h2>
          <div class="lds-ripple">
            <div></div>
            <div></div>
          </div>
        </div>
      </section>
      <section v-if="mmrData != null && mmrData.length != 0 && !loading">
        <div class="box">
          <hr />
          <label>
            My MMR
          </label>
          <hr />
        </div>
        <br />
        <div class="app">
          <div>
            <h1 class="mode">Ranked</h1>
            <template v-if="mmrData.ranked.avg">
              <img
                :src="require(`@/assets/img/${srcImg}`)"
                alt="Rank"
                width="40%"
              />
              <h2>
                MMR: <strong>{{ mmrData.ranked.avg }}</strong>
              </h2>
              <h2 v-html="mmrData.ranked.summary"></h2>
            </template>
            <template v-else>
              <h2>MMR : NA / Not enough game</h2>
            </template>
          </div>
          <div>
            <h1 class="mode">Normal</h1>
            <template v-if="mmrData.normal.avg">
              <h2>
                MMR: <strong>{{ mmrData.normal.avg }}</strong>
              </h2>
            </template>
            <template v-else>
              <h2>MMR : NA / Not enough game</h2>
            </template>
          </div>
          <div>
            <h1 class="mode">Aram</h1>
            <template v-if="mmrData.ranked.avg">
              <h2>
                MMR: <strong>{{ mmrData.ranked.avg }}</strong>
              </h2>
            </template>
            <template v-else>
              <h2>MMR : NA / Not enough game</h2>
            </template>
          </div>
        </div>
      </section>
    </div>
    <footer>
      <div style="text-align:center;">
        <p>
          API Provided by <a href="https://dev.whatismymmr.com/">WhatIsMyMMR</a>
        </p>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loading: false,
      summonerName: '',
      region: 'euw',
      regions: [
        { text: 'EUW', value: 'euw' },
        { text: 'NA', value: 'na' },
        { text: 'EUNE', value: 'eune' }
      ],
      mmrData: [],
      cors_anywhere: 'https://cors-anywhere.herokuapp.com/',
      base_api_url:
        'https://region.whatismymmr.com/api/v1/summoner?name=summonerName',
      errorMessage: '',
      mmrSummary: '',
      srcImg: 'unranked.png'
    };
  },
  methods: {
    fetchData() {
      this.loading = true;
      fetch(this.apiUrl)
        .then(response => {
          const contentType = response.headers.get('content-type');
          if (!contentType || !contentType.includes('application/json')) {
            throw new TypeError("Oops, we haven't got JSON!");
          }
          return response.json();
        })
        .then(data => {
          if (!Object.keys(data).includes('error')) {
            this.mmrData = data;
            this.mmrSummary = data.ranked.summary || '';
            if (this.mmrSummary.includes('Gold IV')) {
              this.srcImg = 'gold_4.png';
            } else if (this.mmrSummary.includes('Gold III</b>')) {
              this.srcImg = 'gold_3.png';
            } else if (this.mmrSummary.includes('Gold II</b>')) {
              this.srcImg = 'gold_2.png';
            } else if (this.mmrSummary.includes('Gold I</b>')) {
              this.srcImg = 'gold_1.png';
            } else if (this.mmrSummary.includes('Silver IV')) {
              this.srcImg = 'silver_4.png';
            } else if (this.mmrSummary.includes('Silver III</b>')) {
              this.srcImg = 'silver_3.png';
            } else if (this.mmrSummary.includes('Silver II</b>')) {
              this.srcImg = 'silver_2.png';
            } else if (this.mmrSummary.includes('Silver I</b>')) {
              this.srcImg = 'silver_1.png';
            } else if (this.mmrSummary.includes('Bronze IV')) {
              this.srcImg = 'bronze_4.png';
            } else if (this.mmrSummary.includes('Bronze III</b>')) {
              this.srcImg = 'bronze_3.png';
            } else if (this.mmrSummary.includes('Bronze II</b>')) {
              this.srcImg = 'bronze_2.png';
            } else if (this.mmrSummary.includes('Bronze I</b>')) {
              this.srcImg = 'bronze_1.png';
            } else if (this.mmrSummary.includes('Iron IV')) {
              this.srcImg = 'iron_4.png';
            } else if (this.mmrSummary.includes('Iron III</b>')) {
              this.srcImg = 'iron_3.png';
            } else if (this.mmrSummary.includes('Iron II</b>')) {
              this.srcImg = 'iron_2.png';
            } else if (this.mmrSummary.includes('Platinum I</b>')) {
              this.srcImg = 'platinum_1.png';
            } else if (this.mmrSummary.includes('Platinum IV')) {
              this.srcImg = 'platinum_4.png';
            } else if (this.mmrSummary.includes('Platinum III</b>')) {
              this.srcImg = 'platinum_3.png';
            } else if (this.mmrSummary.includes('Platinum II</b>')) {
              this.srcImg = 'platinum_2.png';
            } else if (this.mmrSummary.includes('Platinum I</b>')) {
              this.srcImg = 'platinum_1.png';
            } else if (this.mmrSummary.includes('Diamond IV')) {
              this.srcImg = 'diamond_4.png';
            } else if (this.mmrSummary.includes('Diamond III</b>')) {
              this.srcImg = 'diamond_3.png';
            } else if (this.mmrSummary.includes('Diamond II</b>')) {
              this.srcImg = 'diamond_2.png';
            } else if (this.mmrSummary.includes('Diamond I</b>')) {
              this.srcImg = 'diamond_1.png';
            } else if (this.mmrSummary.includes('Challenger</b>')) {
              this.srcImg = 'orichall.png';
            } else if (this.mmrSummary.includes('Master</b>')) {
              this.srcImg = 'orimaster.png';
            } else {
              this.srcImg = 'unranked.png';
            }
          } else {
            this.errorMessage = 'No Data Found';
          }

          this.loading = false;
        })
        .catch(error => {
          console.error(error);
          this.errorMessage = error;
          this.bLoading = false;
        });
    }
  },
  computed: {
    apiUrl() {
      let url =
        this.cors_anywhere +
        this.base_api_url
          .replace('region', this.region)
          .replace('summonerName', this.summonerName);
      return url;
    }
  }
};
</script>

<style></style>
