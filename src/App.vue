<template>
  <div class="container">
    <suspense>
      <div class="quote">
        <i>"{{ quote }}"</i>
      </div>
    </suspense>
    <span id="author">- Kanye West</span>
    <span id="refresh"
      >Refresh for more wisdom from
      <span class="authorMr">Mr. Kanye West</span></span
    >
  </div>
</template>

<script>
import { ref } from "vue";
import KanyeAPI from "./services/KanyeAPI";

export default {
  async setup() {
    const quote = ref("");

    const loadQuote = async () => {
      try {
        const response = await KanyeAPI.getQuote(); // <--- THIS LINE
        quote.value = response.data.quote;
      } catch (err) {
        console.log(err);
      }
    };

    const createPost = async () => {
      const response = await KanyeAPI.createPost(
        JSON.stringify({
          title: "foo",
          body: "bar",
          userId: 1,
        })
      );
      console.log(response);
    };

    loadQuote();

    return {
      createPost,
      quote,
    };
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Comic+Neue:ital,wght@0,400;0,700;1,400;1,700&display=swap");
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  .quote {
    i {
      font-size: 1.5rem;
      max-width: 40ch;
      text-align: center;
      justify-content: center;
      font-weight: 700;
      font-style: italic;
      font-family: "Comic Neue", cursive;
      margin-bottom: 30px;
    }
  }
  span {
    margin: 15px 0 8px 0;
  }
  #refresh {
    font-size: 12px;
  }
  #author {
    font-size: 1.2rem;
  }
  .authorMr {
    font-weight: bold;
    text-decoration: underline;
    text-underline-offset: 4px;
  }
}
</style>
