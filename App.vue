<script>
	// 导入统一身份信息管理模块
	import uniIdPagesInit from '@/uni_modules/uni-id-pages/init.js'
	// #ifdef VUE3
	// 导入uniIm的Utils工具类
	import uniIm from '@/uni_modules/uni-im/sdk/index.js'
	// 引入扩展插件
	import MsgReaderExtension from '@/uni_modules/uni-im-msg-reader/extension.js'
	// #endif
	import {
		store,
		mutations
	} from '@/uni_modules/uni-id-pages/common/store.js'

	export default {
		onLaunch: async function() {
			console.log('App Launch')
			// #ifdef MP-WEIXIN
			this.updateVersion()
			// #endif
			// #ifdef VUE3
			// 安装uniIm扩展插件
			MsgReaderExtension.install()
			// #endif
			// 初始化uni身份信息管理模块
			uniIdPagesInit()
			// #ifdef VUE3
			// 初始化uniIm
			uniIm.init()
			// #endif
			if (store.hasLogin) {
				mutations.updateUserInfo()
			}
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		},
		methods: {
			updateVersion() {
				const updateManager = uni.getUpdateManager()
				updateManager.onCheckForUpdate((res) => {
					// 请求完新版本信息的回调
					if (res.hasUpdate) {
						console.log('有新版本')
					}
				})
				updateManager.onUpdateReady(() => {
					updateManager.applyUpdate()
				})
				updateManager.onUpdateFailed(() => {
					// 新版本下载失败
					console.log('download error')
				})
			}
		}
	}
</script>

<style lang="scss">
	/*每个页面公共css */
	button[type=primary] {
		background-color: #55aaff;
	}

	button[type=primary]::hover {
		background-color: #55aaff;
	}

	/* #ifdef H5 */
	@media screen and (min-width:960px) {

		/* pc宽屏 隐藏会话页面头部 && 全局底部导航 以下兼容了Vue2和3两种模式的样式*/
		uni-page[data-page="uni_modules/uni-im/pages/index/index"] uni-page-head,
		.uni_modules-uni-im-pages-index-index uni-page-head,
		uni-tabbar,
		.uni-app--showtabbar uni-page-wrapper::after {
			display: none !important;
		}
	}

	/* #endif */

	/* #ifndef APP-NVUE */
	@import "@/uni_modules/uni-im/static/iconfont.css";
	/* #endif */
</style>