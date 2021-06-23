<template>
  <div id="store1">store</div>
  <div id="ip1" ref="ip1"></div>
  <div id="ip2" ref="ip2"></div>
  <div id="area1">area 1</div>
  <div id="jail">jail</div>
  <div id="area2">area2</div>
  <div id="area3">area3</div>
  <div id="store2">store2</div>
  <div id="area4">area4</div>
  <div id="lucky1">lucky1</div>
  <div id="area5">area5</div>
  <div id="store3">store3</div>
  <div id="area6">area6</div>
  <div id="hospital">hospital</div>
  <div id="area7">area7</div>
  <div id="lucky2">lucky2</div>
  <div id="area8">area8</div>
  <div id="store4">store4</div>
  <div id="area9">area9</div>
  <div id="bank">bank</div>
  <div id="area10">area10</div>

  <button @click="randomDice()">hi</button>
  <div id="userInfo">
    <p>我是{{curUserInfo.username}}</p>
    <p>我有{{curUserInfo.houseCount}}处房产哦</p>
    <p>我兜里还有{{curUserInfo.moneySum}}元哦</p>
  </div>
  <div id="areaInfo" ref="areaInfo">
    <p>这是{{curItemInfo.areaName}}哦</p>
  </div>

</template>

<script>
  import {
    reactive,
    ref,
    watchEffect
  } from 'vue';

  export default ({
    components: {

    },
    setup() {
      const ip1 = ref(null);
      const ip2 = ref(null);
      const ip1Step = ref(0);
      const ip2Step = ref(0);
      const areaInfo = ref(null);
      const suspendTimes = ref(null);
      const curIp = ref(null);
      curIp.value = 'ip1';
      const areaAddressList = [{
          gridarea: '1/1/1/2',
          areaname: "store 1",
        },
        {
          id: '1',
          gridarea: '1/2/1/3',
          areaname: 'area1 的第一块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '1',
          gridarea: '1/3/1/4',
          areaname: 'area1 的第二块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '1',
          gridarea: '1/4/1/5',
          areaname: 'area1 的第三块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'

        },
        {
          gridarea: '1/5/1/6',
          areaname: 'jail',
          suspended: 2
        },
        {
          id: '2',
          gridarea: '1/6/1/7',
          areaname: 'area2 的第一块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '2',
          gridarea: '1/7/1/8',
          areaname: 'area2 的第二块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '3',
          gridarea: '1/8/1/9',
          areaname: 'area3 的第一块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '3',
          gridarea: '1/9/1/10',
          areaname: 'area3 的第二块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '3',
          gridarea: '1/10/1/11',
          areaname: 'area3 的第三块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '3',
          gridarea: '1/11/1/12',
          areaname: 'area3 的第四块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          gridarea: '1/12/1/13',
          areaname: 'store 2',
        },
        {
          id: '4',
          gridarea: '2/12/2/13',
          areaname: 'area4 的第一块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '4',
          gridarea: '3/12/3/13',
          areaname: 'area4 的第二块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '4',
          gridarea: '4/12/4/13',
          areaname: 'area4 的第三块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          gridarea: '5/12/5/13',
          areaname: 'lucky1'
        },
        {
          id: '5',
          gridarea: '6/12/6/13',
          areaname: 'area5 的第一块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '5',
          gridarea: '7/12/7/13',
          areaname: 'area5 的第二块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '5',
          gridarea: '8/12/8/13',
          areaname: 'area5 的第三块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          gridarea: '9/12/9/13',
          areaname: 'store3'
        },
        {
          id: '6',
          gridarea: '9/11/9/12',
          areaname: 'area6 的第一块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '6',
          gridarea: '9/10/9/11',
          areaname: 'area6 的第二块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '6',
          gridarea: '9/9/9/10',
          areaname: 'area6 的第三块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          gridarea: '9/8/9/9',
          areaname: 'hospital',
          suspended: 1
        },
        {
          id: '7',
          gridarea: '9/7/9/8',
          areaname: 'area7 的第一块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '7',
          gridarea: '9/6/9/7',
          areaname: 'area7 的第二块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '7',
          gridarea: '9/5/9/6',
          areaname: 'area7 的第三块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '7',
          gridarea: '9/4/9/5',
          areaname: 'area7 的第四块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          gridarea: '9/3/9/4',
          areaname: 'lucky2'
        },
        {
          id: '8',
          gridarea: '9/2/9/3',
          areaname: 'area8 ',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          gridarea: '9/1/9/2',
          areaname: 'store3'
        },
        {
          id: '9',
          gridarea: '8/1/8/2',
          areaname: 'area9 的第四块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '9',
          gridarea: '7/1/7/2',
          areaname: 'area9 的第三块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '9',
          gridarea: '6/1/6/2',
          areaname: 'area9 的第二块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '9',
          gridarea: '5/1/5/2',
          areaname: 'area9 的第一块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          gridarea: '4/1/4/2',
          areaname: 'bank'
        },
        {
          id: '10',
          gridarea: '3/1/3/2',
          areaname: 'area10 的第二块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        },
        {
          id: '10',
          gridarea: '2/1/2/2',
          areaname: 'area10 的第一块地',
          moneyCost: '1000',
          owner: undefined,
          houseLevel: '0'
        }
      ];
      const userData = [{
          username: '小啵',
          houseCount: 0,
          moneySum: 10000
        },
        {
          username: '胖啵',
          houseCount: 0,
          moneySum: 10000
        }
      ]
      const curItemInfo = reactive({
        areaAddr: '1/1/1/2',
        areaName: 'store1'
      })
      const curUserInfo = reactive({
        username: '小啵',
        houseCount: 0,
        moneySum: 10000
      })
      const randomDice = () => {

        const userOne = curIp.value === 'ip1' ? window.getComputedStyle(ip1.value) : window.getComputedStyle(ip2
          .value);
          const temp = stepRandom();
        const gridArr = moveItem(userOne,temp);
       addStep(curIp.value,temp);
        curIp.value === 'ip1' ? ip1.value.style.gridArea = gridArr.join('/').toString() : ip2.value.style.gridArea =
          gridArr.join('/').toString();

        updateAreaInfo();
        updateUserInfo(curIp.value === 'ip1' ? 0 : 1);
        //我猜我当时想通过这个来搞监狱停留
        if(suspendTimes.value){
          curIp.value === 'ip1' ? curIp.value = 'ip2' : curIp.value = 'ip1';
          suspendTimes.value--;
        }
        curIp.value === 'ip1' ? curIp.value = 'ip2' : curIp.value = 'ip1';

      }
      function stepRandom(){
        return Math.floor(Math.random() * 6 + 1);
      }

      function moveItem(userOne,temp) {
       
        
        let gridStr = userOne.gridArea;
        let gridArr = gridStr.split('/');
        if (userOne.gridRowEnd === '1') {
          let tempColumn = parseInt(gridArr[3]) + temp;
          if (userOne.gridRowEnd === '1') {
            if (tempColumn < 14) {
              gridArr[3] = tempColumn
              gridArr[1] = tempColumn - 1;
            } else {
              let diffColumn = tempColumn - 13;
              gridArr[1] = 12;
              gridArr[3] = 13;
              gridArr[0] = parseInt(gridArr[0]) + diffColumn;
              gridArr[2] = gridArr[0]
            }
          }

        } else if (userOne.gridColumnEnd === '13' && userOne.gridRowStart != '9') {
          let tempRow = parseInt(gridArr[2]) + temp;
          if (tempRow < 10) {
            gridArr[2] = tempRow;
            gridArr[0] = tempRow;
          } else {
            let diffRow = tempRow - 8;
            gridArr[0] = 9;
            gridArr[2] = 9;
            gridArr[1] = 13 - diffRow;
            gridArr[3] = gridArr[1] + 1;
          }
        } else if (userOne.gridRowStart === '9' && userOne.gridColumnStart != "1") {
          let tempColumn = parseInt(gridArr[1]) - temp;
          if (tempColumn > 0) {

            gridArr[1] = tempColumn;
            gridArr[3] = gridArr[1] + 1;
          } else {
            gridArr[0] = 9 + tempColumn - 1;
            gridArr[2] = gridArr[0];
            gridArr[1] = 1;
            gridArr[3] = 2;
          }
        } else {
          let tempRow = parseInt(gridArr[0]) - temp;
          if (tempRow > 0) {
            gridArr[0] = tempRow;
            gridArr[2] = tempRow;
          } else {
            gridArr[0] = 1;
            gridArr[2] = 1;
            gridArr[1] = (-tempRow) + 2;
            gridArr[3] = gridArr[1] + 1
          }
        }
        return gridArr;
      }

      function updateAreaInfo() {
        curItemInfo.areaAddr = curIp.value === 'ip1' ? ip1.value.style.gridArea.replaceAll(' ', '') : ip2.value.style
          .gridArea.replaceAll(' ', '')
        areaAddressList.forEach((item) => {
          if (item.gridarea === curItemInfo.areaAddr) {
            curItemInfo.areaName = item.areaname
            if (item.id) {
              if (!item.owner) {
                areaInfo.value.innerHTML =
                  `
            
              <p>这块${item.areaname}价值${item.moneyCost}</p>
              <button id="buy-btn" data-id="${item.id}">买下它</button>`
              } else {
                if (curUserInfo.username !== item.owner && item.houseLevel === '0') {

                  areaInfo.value.innerHTML =
                    `<p>这块地已经是你的了 想要盖个小房子吗</p> <button id="house-btn" data-areaname="${item.areaname}">盖个房子吧</button>`
                } else if (curUserInfo.username !== item.owner && (item.houseLevel == '1' || item.houseLevel ==
                    '2')) {
                  
                  areaInfo.value.innerHTML =
                    `<p>你的小房子很可爱，把它变成大房子吧</p> <button id="house-btn" data-areaname="${item.areaname}">变成大房子</button>`
                
                } else if (curUserInfo.username !== item.owner && item.houseLevel == '3') {
                  `<p>这是个漂亮的大别野 不能再升级啦！！！</p> `
                } else if (curUserInfo.username === item.owner && item.houseLevel !== '0') {
                  areaInfo.value.innerHTML =
                    `
              <p>这块地是${item.owner}的了</p>
              <p>交点房租吧小倒霉蛋</p>
              <button id="liveHouse-btn" data-level="${item.houseLevel}">${item.houseLevel}</button>`

                } else {

                  areaInfo.value.innerHTML =
                    `
              <?>这块地是${item.owner}的了</?>`
                }


              }
            } else {
              if (item.suspended) {
                areaInfo.value.innerHTML =
                  `
              <p>这是${item.areaname}哦</p>
              <p>停留${item.suspended}吧！</p>
              `
              
                suspendTimes.value = item.suspended + 1
                curIp.value === 'ip1' ? curIp.value = 'ip2' : curIp.value = 'ip1';
              } else {
                
                areaInfo.value.innerHTML =
                  `
              <p>这是${item.areaname}哦</p>
              `
              }

            }
          }
        })
      }

      function updateUserInfo(ipIndex) {

        curUserInfo.username = userData[ipIndex].username;
        curUserInfo.houseCount = userData[ipIndex].houseCount;
        curUserInfo.moneySum = userData[ipIndex].moneySum;
      }

      function addStep(ip,temp){
        if(ip==='ip1'){
          const furtureStep = temp + ip1Step.value
          if(ip1Step.value < 35 && furtureStep > 34){
            userData[0].moneySum += 1000;
          }
          if(furtureStep<38){
             ip1Step.value = furtureStep
          }else{
            ip1Step.value = furtureStep - 38
          }
         
        }else{
         const furtureStep = temp + ip2Step.value
         if(ip2Step.value < 35 && furtureStep > 34){
            userData[1].moneySum += 1000;
          }
          if(furtureStep<38){
             ip2Step.value = furtureStep
          }else{
            ip2Step.value = furtureStep - 38
          }
        }
        console.log(ip,temp,ip1Step.value,ip2Step.value)
      }


      document.body.onclick = function (ev) {
        if (ev.target.id === 'buy-btn') {
          const areaId = ev.target.dataset.id;
          let ipIndex = curIp.value === 'ip1' ? 1 : 0;
          const queryItemList = areaAddressList.filter((item) => {
            return item.id === areaId
          })

          if (parseInt(userData[ipIndex].moneySum) > parseInt(queryItemList[0].moneyCost)) {
            userData[ipIndex].moneySum = parseInt(userData[ipIndex].moneySum) - parseInt(queryItemList[0]
              .moneyCost)
            areaAddressList.forEach((item) => {
              if (item.id === areaId) {
                item.owner = userData[ipIndex].username;
              }
            })

            areaInfo.value.innerHTML =
              `
              <p>谢谢老板🙏</p>
              `
            updateUserInfo(ipIndex)
          } else {
            areaInfo.value.innerHTML =
              `
              <p>你买不起哦小穷光蛋</p>
              `
          }

        } else if (ev.target.id === 'house-btn') {
          const areaname = ev.target.dataset.areaname;
          const queryItemList = areaAddressList.filter((item) => {
            return item.areaname === areaname
          })
          let ipIndex = curIp.value === 'ip1' ? 1 : 0;
          if (parseInt(userData[ipIndex].moneySum) > 200 && queryItemList[0].houseLevel == '0') {
            userData[ipIndex].moneySum = parseInt(userData[ipIndex].moneySum) - 200;
            queryItemList[0].houseLevel = parseInt(queryItemList[0].houseLevel) + 1
            const firstHouse = document.createElement('div');
            firstHouse.classList.add('firstHouse');
            firstHouse.dataset.areaname = queryItemList[0].areaname;
            firstHouse.style.gridArea = queryItemList[0].gridarea;

            document.getElementById('container').appendChild(firstHouse);
            areaInfo.value.innerHTML =
              `
             <p>${userData[ipIndex].username}🈶️小房子啦</p>
              `
            userData[ipIndex].houseCount = parseInt(userData[ipIndex].houseCount) + 1;
            updateUserInfo(ipIndex)
          } else if(parseInt(userData[ipIndex].moneySum) > 200*parseInt(queryItemList[0].houseLevel) && queryItemList[0].houseLevel != '0'){
            const curLevel = queryItemList[0].houseLevel;
            const houseArr = curLevel == 1 ?document.getElementsByClassName('firstHouse') : document.getElementsByClassName('middleHouse');
           let curHouse = [];
           houseArr.forEach((item)=>{
             if(item.dataset.areaname === queryItemList[0].areaname){
               curHouse.push(item)
             }
           })
            
            userData[ipIndex].moneySum = parseInt(userData[ipIndex].moneySum) - parseInt(curLevel)*200;
            curLevel == 1 ? curHouse[0].classList.add('middleHouse') : curHouse[0].classList.add('finalHouse');
            queryItemList[0].houseLevel = parseInt(queryItemList[0].houseLevel) + 1
            updateUserInfo(ipIndex)

          }
          else {
            areaInfo.value.innerHTML =
              `
             <p>${userData[ipIndex].username}钱不够啦</p>
              `
          }
        } else if (ev.target.id === 'liveHouse-btn') {
          const houseLevel = ev.target.dataset.level;
          let ipIndex = curIp.value === 'ip1' ? 1 : 0;
          let ip2Index = curIp.value === 'ip1' ? 0 : 1;
          if (parseInt(userData[ipIndex].moneySum) > houseLevel * 200) {
            userData[ipIndex].moneySum = parseInt(userData[ipIndex].moneySum) - 200 * houseLevel;
            userData[ip2Index].moneySum = parseInt(userData[ip2Index].moneySum) + 200 * houseLevel;
            areaInfo.value.innerHTML =
              `
             <p>谢谢老板哦！</p>
              `
            updateUserInfo(ip2Index);
            updateUserInfo(ipIndex);
          } else {
            areaInfo.value.innerHTML =
              `
             <p>游戏结束！你输啦</p>
              `
          }
        }

      }
      watchEffect(()=>{
        if(ip1Step.value)
        console.log(ip1Step.value,ip2Step.value)
      })


      return {
        randomDice,
        ip1,
        ip2,
        areaInfo,
        curItemInfo,
        curUserInfo
      }
    },
  })
</script>

<style>
  .firstHouse {
    width: 100px;
    height: 100px;
    border-radius: 50%;
    background: transparent;
    background-image: url('../assets/房子16.png');
    background-size: 50px 50px;
    background-repeat: no-repeat;
    z-index: 101;
    top: 25px;
    left: 25px;
    position: relative;
  }
  .middleHouse {
   width: 100px;
    height: 100px;
    border-radius: 50%;
    background: transparent;
    background-image: url('../assets/房子7.png');
    background-size: 50px 50px;
    background-repeat: no-repeat;
    z-index: 101;
    top: 25px;
    left: 25px;
    position: relative;
  }

  .finalHouse {
   width: 100px;
    height: 100px;
    border-radius: 50%;
    background: transparent;
    background-image: url('../assets/房子3.png');
    background-size: 50px 50px;
    background-repeat: no-repeat;
    z-index: 101;
    top: 25px;
    left: 25px;
    position: relative;
  }
  #ip1 {
    width: 100px;
    height: 100px;
    grid-area: 1/1/1/2;
    background: transparent;
    z-index: 200;
    position: relative;
    background-image: url('../assets/Boy-04.png');
    background-repeat: no-repeat;
    background-size: 68.4px 90px;
    top: 5px;
    left: 15.8px;

  }

  #ip2 {
     width: 100px;
    height: 100px;
    grid-area: 1/1/1/2;
    background: transparent;
    z-index: 200;
    position: relative;
    background-image: url('../assets/Girl-03.png');
    background-repeat: no-repeat;
    background-size: 68.4px 90px;
    top: 5px;
    left: 15.8px;
  }

  #container {
    width: 1200px;
    height: 900px;
    position: relative;
    top: 80px;
    margin: 0 auto;
    display: grid;
    grid-template-rows: repeat(9, 100px);
    grid-template-columns: repeat(12, 100px);
    background: lightblue;
    text-align: center;
    line-height: 100px;
  }

  #center {
    grid-area: 2/2/9/12;
    background: lightgoldenrodyellow;
  }

  #userInfo {
    grid-area: 5/2/9/7;
    background: rosybrown;
  }

  #areaInfo {
    grid-area: 5/7/9/12;
    background: dodgerblue;
  }

  #store1 {
    grid-area: 1/1/1/2;
    background: lightcoral;
  }

  #area1 {
    grid-area: 1/2/1/5;
    background: lightsalmon;
  }

  #jail {
    grid-area: 1/5/1/6;
    background: lightcyan;
  }

  #area2 {
    grid-area: 1/6/1/8;
    background: lightgreen;
  }

  #area3 {
    grid-area: 1/8/1/12;
    background: lightskyblue;
  }

  #store2 {
    grid-area: 1/12/1/13;
    background: lightcoral;
  }

  #area4 {
    grid-area: 2/12/5/13;
    background: lightseagreen;
  }

  #lucky1 {
    grid-area: 5/12/6/12;
    background: lightyellow;
  }

  #area5 {
    grid-area: 6/12/9/12;
    background: lightgrey;
  }

  #store3 {
    grid-area: 9/12/9/13;
    background: lightcoral;
  }

  #area6 {
    grid-area: 9/9/9/12;
    background: lightpink;
  }

  #hospital {
    grid-area: 9/8/9/9;
    background: whitesmoke;
  }

  #area7 {
    grid-area: 9/4/9/8;
    background: limegreen;
  }

  #lucky2 {
    grid-area: 9/3/9/4;
    background: lightyellow;
  }

  #area8 {
    grid-area: 9/2/9/3;
    background: mediumpurple;
  }

  #store4 {
    grid-area: 9/1/9/2;
    background: lightcoral;
  }

  #area9 {
    grid-area: 5/1/9/2;
    background: mediumaquamarine;
  }

  #bank {
    grid-area: 4/1/5/2;
    background: lightyellow;
  }

  #area10 {
    grid-area: 2/1/4/2;
    background: honeydew;
  }
</style>