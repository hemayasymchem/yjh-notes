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

### setState获取最新的state值 使用第二个参数 this.setState({}, cb)
```
this.setState({
  info: 'newInfo'
}, () => {
  console.log(this.state.info, '这里看看最新的state')
})
```

### 子组件默认一些变量和方法的解决方式 staticProps
```
calss childComponent extends React.Component {
  statis defaultProps = { // 这里就是默认的方法和属性的值
    setFocusFun: () => {},
    searchBlur: () => {},
    searchValue: ''
  }

  static propTyps = { // 还可以做校验 PropTypes是引入的插件
    setIsFocus: PropTypes.func,
    searchFun: PropTypes.func,
    searchValue: PropTypes.string.isRequired // 还可以链式加规则
  }
  /**
  * 子组件里尽量不做任何处理方式 都放到父组件去处理
    this.props.func()
  */
}
```

### 组件使用多个类名
```
 <div className={`calss1 class2 class3-${isFocus ? 'focus' : 'none'}`}></div>
```
### jsx不能用if else 但是可以用三目
```
<div>
{isFocus ? 
 (<h1>真</h1>)
 : (<h1>假</h1>)
}
</h1>

图片的使用
<img src={`${this.state.info ? demo1 : demo2}`}> 两个变量卸载${} 花括号里面
```
