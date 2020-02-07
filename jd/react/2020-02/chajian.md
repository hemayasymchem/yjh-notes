2020-02-06 项目中和学习中使用到的npm包

### 项目中的
```
react-loadable
  react路由中使用 异步加载组件（分割代码） 并且可以设置一个过场组件

ramda 一款实用的 JavaScript 函数式编程库  
```










### 学习中的






### 项目中的某些代码部分


##### 音频需要特殊处理 amr格式
```
  export const readBlob = (blob, callback) => {
    let promise = new Promise((reslove, reject) => {
      let reader = new fileReader()
      reader.onload = function(e) {
        let data = new Uint8Array(e.target.result)
        reslove(data)
      }
    })
    return promise
  }

  export const getVoice = (url = "") => {
  return fetch(url, {
    credentials: "include",
    responseType: "blob" //arraybuffer
  })
    .then(res => {
      return res.blob();
    })
    .then(blob => {
      return readBlob(blob).then(data => {
        let samples = AMR.decode(data);

        if (!samples) {
          return Promise.reject("Failed to decode!");
        }
        return samples;
      });
    })
    .catch(err => {
      return Promise.reject(err);
    });
};

/**
 * 图片系统 上传
 */
 export const uploadImage = (file, onProgress) => {
  return new Promise((resolve, reject) => {
    let reader = new FileReader();
    reader.readAsBinaryString(file);
    reader.onload = async event => {
      let crc = CRC32.bstr(event.target.result);
      let keycode = `${crc.toString(16)}${file.size}_`;

      const xhr = new XMLHttpRequest();
      xhr.open("POST", "/imageUpload.action");
      xhr.setRequestHeader("content-type", file.type);
      xhr.setRequestHeader("keycode", keycode);
      xhr.onload = e => {
        const json = JSON.parse(e.target.responseText);
        // console.log(json);
        resolve(json);
      };
      xhr.onerror = e => {
        reject(e);
      };
      xhr.upload.onprogress = onProgress;
      xhr.send(file);
    };
  });
}; 
```

### ant_mall request方法
```
export const request = function(functionId, method="GET', params_json=undefined) {
  let BASE_URL = '//api.healthid.com/client'
  if (process.env.NODE_ENV === 'development') {
    BASE_URL = '/client'
  }
  let URL = BASE_URL
  let wq = wxTools.isQQ() || wxTools.isWeixin()
  let payload = {
    funtionId,
    body: JSON.stringify(params_json),
    loginType: wq ? 1 : undefined,
    loginWQBiz: wq ? 'nethp-diag' : undefined
  }

  let options = {
    methods,
    headers: {
      'Content-Type": 'appliation/x-www-form-urlencode',
      Accept: 'applation/json, text/plain, */*'
    },
    creditals: 'include'
  }

  if (options.method === "GET") {
    let query  = qs.stringify(payload),
     URL = `${BASE_URL}?${query}`
  }
  if (options.method == "POST") {
    const contentType = options.headers["Content-Type"].toLowerCase();
    if (contentType == "application/json") {
      options.body = JSON.stringify(payload);
    }

    if (contentType == "application/x-www-form-urlencoded") {
      options.body = qs.stringify(payload);
    }
  }

  return fetch(URL, options)
    .then(checkStatus)
    .then(parseJSON)
    .then(res => {
      if (res.code !== "0000") {
        // if (res.code == "1023") {
        //   toLogin(window.location.href);
        //   return;
        // }
        // 请求的唯一标示
        const excludes = [
          'gwSystem/isLogin'
        ];
        if (!excludes.includes(functionId)) {
         Toast.error(res.msg);
        }
      }

      return res;
    })
    .catch(err => {
      throw "The fucking code，响应异常了，贞操蛋";
    });
};
}

```


