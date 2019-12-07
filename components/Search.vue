<template>
  <div class="search">
    <div class="input">
      <input
        type="text"
        placeholder="search through projects"
        v-model="search"
        v-on:keyup="getResults"
      />
    </div>
    <div class="view">
      <ul>
        <li v-for="(project, i) in results.slice().reverse()" v-bind:key="i">
          <!-- <div class="thumbnail"></div> -->
          <div class="info">
            <h3>{{project.name}}</h3>
            <p>{{project.description}}</p>
            <p>
              website:
              <a target="_blank" v-bind:href="project.website">{{project.website}}</a>
            </p>
            <button v-on:click="getWebview(project.website)">preview</button>
            <span>(might not display properly)</span>
          </div>
        </li>
      </ul>
      <object v-bind:data="webview"></object>
    </div>
  </div>
</template>
<script>
import algoliasearch from "algoliasearch";

const client = algoliasearch("4OIJEQ2N4A", "d49fb689bb2a589acebdbf7311de50c9");
const index = client.initIndex("projects");

export default {
  data() {
    return {
      search: "",
      results: [],
      webview: ""
    };
  },
  computed: {
    projects() {
      return this.$store.state.projects;
    }
  },
  methods: {
    getWebview(url) {
      this.webview = url;
    },
    getResults() {
      index
        .search(this.search)
        .then(({ hits }) => {
          this.results = hits;
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  mounted() {
    this.getResults();
    index.saveObjects(this.projects).catch(err => {
      console.log(err);
    });
  }
};
</script>
<style lang="scss" scoped>
$card-color: #f0f0f0;
.search {
  min-height: 100vh;
  // background: purple;
  width: 100vw;
  object {
    // background: black;
    border-radius: 5px;
    min-width: 375px;
    margin: 20px;
    min-height: 80vh;
    flex: 1;
  }
  .input {
    width: 100%;
    height: 60px;
    display: flex;
    justify-content: center;
    align-items: center;
    // background: purple;
  }
  .view {
    flex: 1;
    display: flex;
    // background: purple;
  }
  input {
    height: 40px;
    width: 300px;
    font-size: 17px;
    border: none;
    float: left;
    padding: 10px;
    box-sizing: border-box;
    outline: none;
    border-radius: 5px;
    background: #f0f0f0;
  }
  ul {
    
    width: calc(100% - 50px);
    max-width: 600px;
    min-width: 300px;
    min-height: 100px;
    max-height: calc(100vh - 100px);
    overflow-y: auto;
    padding: 0px 5px 0px 0px;
    flex-direction: column;
    margin: 20px;
    // margin: 0 auto;
    justify-content: center;
    li {
      display: flex;
      min-height: 10px;
      width: calc(100% - 25px);
      box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2);
      margin: 20px;
      margin-bottom: 30px;
      background: $card-color;
      padding: 15px;
      border-radius: 5px;
      &:hover {
        box-shadow: 0px 0px 10px #25ceff;
      }
      .thumbnail {
        width: 100px;
        height: 100px;

        background: green;
        border-radius: 5px;
      }

      .thumbnail:after {
        content: "";
        display: block;
        padding-bottom: 100%;
      }

      .info {
        text-align: left;
        padding-left: 20px;
        flex: 1;
        button {
          height: 40px;
          min-width: 150px;
          border: none;
          background: #25ceff;
          color: white;
          text-transform: uppercase;
          font-weight: bolder;
          margin-top: 10px;
          border-radius: 5px;
        }
        span {
          font-size: 14px;
          // font-weight: lighter;
          color: rgba(0, 0, 0, 0.2);
        }
        p {
          font-size: calc(10px + 0.5vw);
          max-width: 500px;
        }
      }
    }
  }
}

@media screen and (max-width: 1216px) {
  object {
    display: none;
  }
  ul {
    margin: 0 auto !important;
    padding-top: 20px !important;
    li {
      width: calc(100% - 15px) !important;
      margin: 0px 0px 30px 10px !important;
      button {
        display: none;
      }
      span {
        display: none;
      }
    }
  }
}
</style>