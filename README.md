# yygg-a2z
### 用去来还是挺丝滑的,感谢你的使用

``` js

	import a2z from "@/components/yygg-a2z/yygg-a2z.vue"
	export default {
		components: {
			a2z
		},
		data() {
			return {
				comList: [{
					"corporate_name": "北京测试111",
					"avatar": "https://mine-yygg.oss-cn-qingdao.aliyuncs.com/upload/111_jpg.jpg",
					"mobile": "欢迎使用我的插件",
					"first": "B", // 组件使用的是这个值
				}],
				new_list_arr: []
			}
		},
		onReady() {
			// 一般在请求数据后使用
			this.$refs.a2z.getDataList()
		},
		methods: {
			/**
			 * 返回整理后的数据
			 * @param {Array} e
			 */
			newListArr(e) {
				console.log(e)
				this.new_list_arr = e
			}
		}
	}
```


``` html
		<a2z ref="a2z" :comList="comList" @new_list_arr="newListArr"></a2z>
```
