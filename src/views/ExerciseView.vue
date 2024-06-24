<template>
    <div class="card-box">
        <div class="card">
            <img src="" alt="">
            <h3>تست</h3>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const name = ref('ali');
const family = ref('reza');

let test = [name.value, family.value]
let result = test.every((item) => { return item.length > 0 })
// console.log(result)



let x = [1, 2, 3, 4, 1];
let obj = new Set(x)
x = Array.from(obj)
console.log(x)



</script>

<style scoped>
.card-box {
    display: flex;
    flex-wrap: wrap;
}

.card {
    width: 100%;
    max-width: 10rem;
    padding: 0.5rem;
    border-radius: 8px;
    box-shadow: 0px 0px 8px 0px rgba(117, 117, 117, 0.75);
}

.card img {
    aspect-ratio: 16 / 9;
    border-radius: 6px;

}

.card h3 {
    text-align: end;
    margin: 0.8rem 0;
}
</style>




6dixQIieUpMTiFkwoqfU5BTC84fWaAs4g8CqzZCzTmsmLGy3EPJnZtqdtdildKe/KP6pFixJ6yCVeSKubOAPi923CtHql9cLpE9xg0ikcX1vayDVZUeUf3JSIHckVPFwU1dxnR76t0Du1OeixEpg8bQ9EL0+Mvd2IYHD+xeK5WUKB/Q15vHonj0QRf2mmVgM4hcY48xQrzUQgTxqCNJhGlMqvmOYkbI9EMVTRjdNFw+ZxvSwdMQ0y2ANxNAGUTqMvtbIWoo/WwORikq2QVK9YxsNa4QtgvDrmllL0aBNkGeGfu50UOxPbIK0mxambq1/iledS8waCNINayB6do0yIS4GE3VCTGQKJ62BpBRvlYq+RoCZ0tbRdaAbgB7JTOGoPmaaCRlm44tuCmBTUvEr0wewNhQjNPSmCqu2Qr5iHcTL0p54s0HfEln2wIUE8sv4YKgbXEkqvhAXSDJTOOMMJFwf0YcYssEHB4JV4FJC8W3XIELxNXCtlD/cHHW7Gg2CPnr+0CaIQ3r76dBWbRCXWnL5HU7ecSl+35zLb4Y4pRdz1m2FdBHiVnMuv9bqy2zFb5x3qoUkkc/bK/LiCaiZgmC5QB6wAPICai2vsr+g6SVYakJ2D7dAPUga7iDntxRfgnpRKvwoOhgdhPrgcbAC6kle5a0IzZi3Wp5MYnEv1KMkUlsEkMtQrzrmBysFcsagwLt0x4N6VkovKhWPLVzcVNaf8aARlEzTKb1Vnae4ZwTScWgkScXX5wG5Cg2nIFgmFT2aNV881cegEdUabo17paIeqx814R/Wd4UU/pXv+gUqAAAAAElFTkSuQmCC"/>
  <img id="i7l2h" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAACXBIWXMAAAsTAAALEwEAmpwYAAADeUlEQVR4nO2YS2gTQRiAf/GBtrSC9bWbWkUP1SpFbDPTShXBq+hJBfGoJw8e9GR9o3ixJ0Hc3bSdTVofEWoVWu1BUmNm0mKamWhVUCt40IOKL1BRwZWNLWxeTXswmcV8MLewzJf//2fm/wFKlCjxnxAMzlzqI5sVg7QphvlM1c0vqmEeAplZHgrNVXRzh6KbHapB3qmGaaUsnfxeqnV4QUZwNLHdS/nLFV3XUzedIWHeA9lATBzHTFj2Wtcfmlzgr8Q2kAlMxecJAUS5tYxczicxatcIyAKi4smEgL3W3BrIGwXFIPtAFhDlB5wC3vsjlqc9MLmAbr5e4veXgww0xGJlmPL3Tonanv78tWCYx0AWEOOnU6IQHrGq80TBvhsWBAKVIAPNVCxGjH9PqYWbU6gFLbABZAFTcckpgCJxq8a8MokAGYEToVkgC02MezAVX50S9QPhHOlDHiha90KQDUz5WaeAvVZd603/58MrNW0+yAgaGqrETLxxCjSGY1Z1Z9dE4Q4omlYGMoOjYnd6FOxUUnTyvsrnqwA3gCi/nS5R29M/Bm6hiXEPYvxDyt1A47+8ww8bwS3gCN+bHgXE+Avv8HAVuAVMRXeGBBWDdaOjc8AN1AtRjpkYTZfAVBCwrBngBprpw9WYik9Z0uk8uAUc5Vsw5T8yJcRxcAs4wvdnpNJfiRNT/UaDps1W9c71RbvJcZS3ZpPAVFzIVxOqaVYpOolPPMVVnRwtSlOEGD+XQ4LkOp3svkHVyYMsPcVbRSMHC/6qxZSfzJpOVERawvFFzt9WtwXnqQYZzNNbPPX4/DsLKoFY4kgOiecNLJFsdOy3k/0AnEJ7ao332qGCDs+ShU3Fz3QJ+9lR29P3SjHIh6luXnVMAFUfOVwwCS9NbMVMfMwWjfo7g9ayju7pCRj22Mb8BIWkMZKoxYw/yiZh9xOrrt6YrsAzKMazAzERyHpCjfcUNZP22CmDszYoFl4W35MrpVCEJ6cd+UY2ih7YBMUEDSWqEeX9uaLReD+WHJ552v3Z5k1vpZm9NjG+CzHxOqdIOGat7r1jedq7nEdpB8jExsjTCkTFmfSRTcqxGxmx1vbdtZb7r9kCO0DaNpXyi4iKb7lEEBVjdcGg3M1SSzi+CFN+ClPxLkMimtgObqEhFitLjvapeJw8taK8tdh7KlGiBPxb/gAwKJWlsMaWKAAAAABJRU5ErkJggg=="/>
  <div id="i0rgg">021-88662021
    <br/>
  </div>
  <div id="iun14">تهران - ولی عصر - پایین تر از میدان ونک - کوچه فوزی - پلاک 4 - طبقه ششم
  </div>
  <div id="ipha7">www.TarazGroup.com
    <br/>
  </div>
</body>


* {
  box-sizing: border-box;
}
body {
  margin: 0;
}
#i5wj{
  color:black;
}
#illwp{
  padding:10px;
}
#i8nm{
  padding:10px;
}
#ixbg{
  padding:10px;
}
#icy1{
  padding:10px;
}
#i9vu3{
  color:black;
}
#il10c{
  color:black;
}
#i7l2h{
  color:black;
}
#i0rgg{
  padding:10px;
}
#iun14{
  padding:10px;
}
#ipha7{
  padding:10px;
}
@media (max-width: 561px){
  #i5wj{
    left:0px;
    top:-1px;
    position:absolute;
    width:109px;
    height:76px;
  }
  #illwp{
    left:281px;
    top:6px;
    position:absolute;
    color:#009dd6;
  }
  #i8nm{
    left:186px;
    top:16px;
    position:absolute;
    color:#009dd6;
  }
  #ixbg{
    left:257px;
    top:44px;
    position:absolute;
    font-size:10px;
  }
  #icy1{
    left:291px;
    top:59px;
    position:absolute;
    font-size:10px;
  }
  #i9vu3{
    left:17px;
    top:745px;
    position:absolute;
    width:20px;
    height:20px;
  }
  #il10c{
    left:17px;
    top:706px;
    position:absolute;
    width:20px;
    height:20px;
  }
  #i7l2h{
    left:17px;
    top:664px;
    position:absolute;
    width:20px;
    height:20px;
  }
  #i0rgg{
    left:43px;
    top:656px;
    position:absolute;
    font-size:12px;
  }
  #iun14{
    left:43px;
    top:698px;
    position:absolute;
    font-size:12px;
  }
  #ipha7{
    left:43px;
    top:737px;
    position:absolute;
    font-size:12px;
  }
}
@media (max-width: 530px){
  #i9vu3{
    left:17px;
    top:618px;
    position:absolute;
    width:20px;
    height:20px;
  }
  #ipha7{
    left:43px;
    top:610px;
    position:absolute;
    width:129.015625px;
    height:35px;
  }
}
@media (max-width: 510px){
  #icy1{
    left:291px;
    top:147px;
    position:absolute;
    width:103.1875px;
    height:31px;
  }
}
