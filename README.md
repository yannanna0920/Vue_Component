### 1. marQuee.vue 组件  
### 在Vue 表格操作中实现图片切换 点击大图实现循环切换

```  

{
 	title: this.$t("detail"),
  	width:200,
    render: (h, param) => {
      return h(
        "div",
        { style: { padding: "6px 0", display: "flex" } }, 
        	[ h(marQuee, {
            props: {
              pictureKey:  param.row.pictures,//图片的地址多个为数组
              width: 40,//在操作栏里的宽度
              height: 40,//在操作栏里的高度
            }
          })
        ]
      );
    }
 }
 
 ```

### 记得注册这个组件哈！

