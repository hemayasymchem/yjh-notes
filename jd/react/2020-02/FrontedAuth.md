### 医院项目 动态路由 (我的理解)
```代码来抄一遍看看
import React, { PureComponent } from 'react'
import { Route , Redirect } from 'react-router-dom'
import queryString from 'query-string'
import loading from '@/components/loading'
import { isLogin, gologin } from '@/utils/login'
import wx from '@/utils/wx'
import routerconfig from './routerConfig.js'
import api from '@/api'
import { isShowWebviewHeader, isOldPeopleClient, isShowTitleBack, setTitleBg } from '@/utils/oldPeopleClient'
import { getNewUserState } from '@/store/actions/newUser'
import { connect } from 'react-redux'
import { widthRouter } from 'react-router-dom'

const NotFound = () => {
    return <div>404</div>
}

class FrontedAuth extends PureComponent {
    constructor(props) {
        super(props)
        this.redireactHander = this.redirectHander.bind(this)
        this.state = {
            redirectFlag: false
        }
    }
    redirecthander() {
        this.setState({
            redireactFlag: true
        })
    }

    async checkLogin() {
        let res = await isLogin().catch(err => {
            if (!err || err.code !== '1023') {
                // 判断老人机环境退出登录
                if (isOldPeopleClient()) {
                    ph.toLogout()
                } else {
                    goLogin(window.location.href)
                }
            }
        })
        if (res && res.code === '1027') {
            if (wx.isWeiXin()) {
                let wxData = {
                    appid: '',
                    url: 'esc.jd.com'
                }
                ...
            }
        }
    }
    
}


```