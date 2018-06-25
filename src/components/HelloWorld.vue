<template>
  <div class="hello">
    <input v-model='keyword' />
    <button @click="search(keyword)">Go</button>
    <p v-if="robotResponse.length > 0">来自图灵机器人的回答：{{robotResponse[0].values.text}}</p>
    <!-- <p v-for="answer in answers" :key="$key" :index="$index">{{answer}}</p> -->
  </div>
</template>

<script>
import axios from 'axios'
// axios.defaults.baseURL = API_PROXY + 'http://openapi.tuling123.com/openapi/api/v2';
axios.defaults.headers.post['Content-Type'] = 'application/json';
export default {
  name: 'HelloWorld',
  data () {
    return {
      keyword: '',
      robotResponse:'',
      answers:''
    }
  },
  methods:{
    search(keyword){
      axios.post('/api/v2', {
          "reqType":0,
            "perception": {
                "inputText": {
                    "text": keyword
                },
                "selfInfo": {
                    "location": {
                        "city": "上海",
                        "province": "上海",
                        // "street": "甜爱路"
                    }
                }
            },
            "userInfo": {
                "apiKey": "514eb9b15c1c41c5a20fec05bdfb36f6",
                "userId": "03375"
            }
        })
        .then((response) => {
          this.robotResponse = response.data.results.filter(item => item.resultType === "text");
          // this.answers = response.data.intent.results[0].values.news
        })
        .catch((error) => {
          console.log(error);
        });
    } 
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
