# vue--timaixs
## 浅谈vuex——以时间轴组件为例
### 主要文件目录
```html
_steps.vue
mapGetter('Wsteps');//获取stroe.js中定义的getter变量的wsteps
// 监测该变量
watch: {
      wSteps (wSteps) {
let children = this.$parent.$children;
children.map((item, index) => {
if( wSteps > index ){
item.isActive = false;
item.isWait = false;
item.isFinish = true;
item.isWill = false;
}
else if (wSteps == index) {
item.isActive = true;
item.isWait = false;
item.isFinish = false;
item.isWill = false;
}
else if ((wSteps + 1) === index ) {
item.isActive = false;
item.isWait = false;
item.isFinish = false;
item.isWill = true;
}
else {
item.isActive = false;
item.isWait = true;
item.isFinish = false;
item.isWill = false;
}
})
}
}
```
```html
_stote.js   
定义变量 state、getter、mutaions
const state = { // 定义变量
step: 0,
total: 0
};
// 通过定义getter来获取mapGetter的变量
const getters = {
wSteps: state => state.step // 定义wSteps函数简化了获取变量的方式，$store.state.step
};
// 定义触发事件
const mutations = {
// 进行下一步的事件
STEP_NEXT(state) {
if (state.step === state.total) { // 如果当前步骤等于总的步骤，step置为零，从头开始循环，否则进行下一步
state.step = 0;
}
else {
state.step = state.step + 1;
}
},
// 返回上一步，如果当前步骤不为零，就回退一步，否则直接跳出
STEP_PRE(state) {
if (!state.step) {
return;
}
state.step = state.step - 1;
},
// 指定到具体的步骤
STEP_NUM(state, step) {
state.step = step;
},
// 计算总的步骤
STEP_TOTAL(state, total) {
state.total = total;
}
};
```
> 这个图有助于大家对vuex 的整体工作流程有一个更形象的理解，如下图
![image](http://github.com/yangmengwen/vue--timaixs/raw/master/image/vue.jpg)
