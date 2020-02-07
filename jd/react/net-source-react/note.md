## 项目中的代码

### 
```
const imgCheck = () => {
  try {
    return (
      document
        .createElement('canvas')
        .toDataURL('image/webp')
        .indexOf('data:image/webp') == 0
    );
  } catch (err) {
    return false;
  }
};
```

### 代码中判断是不是XXAPP的代码现在还不理解
```
let isXXapp = window.JSSDK.Client.isXXApp();
```

### 在window上添加埋点记录
```
 function createExposureFloorLog() {
     if (!window.exposureFloorLog) {
         window.exposureFloorLog = {}
     }
     const exposureFloorMap = {
        '#floor2153': 'MInternetHospital_FindServiceFloorExpo', // 找服务
        '#floor2152': 'MInternetHospital_SearchDisFloorExpo', // 搜疾病
        '#floor2150': 'MInternetHospital_FindMedFloorExpo', // 找药品
        '.main-navs': 'MInternetHospital_GoldIconExpo',
        '.main-entry': 'MInternetHospital_GoldBannerExpo',
    }
     Object.keys(exposureFloorMap).map(item => {
        const floor = document.querySelector(item);
        if (floor) {
        let bcr = floor.getBoundingClientRect();

        if (bcr.top < document.body.clientHeight - 52 && bcr.top > 0) {
            if (!window.exposureFloorLog[item]) {
            console.log(`${item} 曝光`);
            mpingClick(exposureFloorMap[item]);
            window.exposureFloorLog[item] = true;
            }
        }

        if (bcr.top < 0) {
            window.exposureFloorLog[item] = false;
        }
        }
    });
 }
```

### ant-医生部分
```
rem.js
const baseSize = 100 // 1rem = 100
function setRem() {
  const scale = document.documentElement.clientWidt / 750
  document.documentElement.style.fontSize = (baseSize * Math.min(scale, 2)) + 'px'
}
setRem()
window.onresize = function() {
  setRem()
}

index.html下的另一份

setRootFontSize()
function setRoorFontSize() {
  function flexible() {
    var deviceWidth = document.documentElement.clientWidth || document.body.clientWidth
    if (deviceWidth > 750) {
      deviceWidth = 750
    }
    if (devicePixelRatio == 1 ){
      deviceWidth = 375
    }

    var rootFontSize = (deviceWidth / (7.5 / 2 )).toFixed(2) // 这里是以 375为基准
    if (window.orientation == 90 || window.orientation == -90) {
        //横屏
        console.log('横屏')
        document.documentElement.style.fontSize = rootFontSize / 2 + 'px';
      } else {
        //竖屏 默认
        console.log('竖屏')
        document.documentElement.style.fontSize = rootFontSize + 'px';
      }
  }

  window.addEventListener('resize', function () {
    flexible();
  }, false);

  window.addEventListener("onorientationchange" in window ? "orientationchange" : "resize", flexible, false);

  flexible();

  window.addEventListener('resize', function () {
    if (document.activeElement.tagName === 'INPUT') {
      document.activeElement.scrollIntoView({
        behavior: "smooth"
      })
    }
  })
}
```
