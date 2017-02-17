<template>
  <div id="app">
    解読したい文字列：<br>
    <textarea v-model="text"></textarea><br>
    使いたいエンコード：<br>
    <textarea v-model="encodes" id=""></textarea>

    <h1>1path</h1>

    <div v-for="result in results1">
      {{result.text}} <span class="small">{{result.encode}}</span>
    </div>

    <h1>2path</h1>

    <div v-for="result in results2">
      {{result.text}} <span class="small">{{result.encode}}</span>
    </div>
  </div>
</template>

<script>
var Combinatorics = require('js-combinatorics');
var iconv = require('iconv-lite');


export default {
  name: 'app',
  data () {
    return {
      text: "",
      results1: [],
      results2: [],
      encodes: "utf8,utf16-le,utf16-be,ascii,CP932,Shift_JIS,EUC-JP"
    }
  },
  watch: {
    "text": function(n){
      //１段階変換
      this.results1 = Combinatorics.permutation(this.encodes.split(","), 2).map((i)=>{
        let buf = iconv.encode(n, i[0])
        let str = iconv.decode(buf, i[1])
        return {
          encode: i,
          text: str
        }
      })

      //２段階変換
      this.results2 = Combinatorics.permutation(this.encodes.split(","), 4).map((i)=>{
        let buf = iconv.encode(n, i[0])
        let str = iconv.decode(buf, i[1])
        let buf2 = iconv.encode(n, i[2])
        let str2 = iconv.decode(buf2, i[3])

        return {
          encode: i,
          text: str2
        }
      })
    }
  }
}
</script>

<style>
.small{
  font-size: 0.5rem;
  color: #AAA;
}
textarea{
  width: 90%;
  height: 100px;
}
</style>
