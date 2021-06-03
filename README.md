#car-number-input
vue h5 车牌号输入


```js
npm install car-number-input -s


<script>
import zCarNumInput from "car-number-input"
export default{
  components:{
    zCarNumInput
  },
  methods:{
    getNum(e){
      console.log(e)
    }
  }
}
</script>
```
<z-car-num-input @carNum="getNum"></z-car-num-input>