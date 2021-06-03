#car-number-input
vue h5 车牌号输入

npm install car-number-input -s

<z-car-num-input @carNum="getNum"></z-car-num-input>

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
