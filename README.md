<p align="center">
  一个灵活的级联选择器
</p>

<p align="center">
  <br>
  <b>生命不息，轮子不止</b>
  <br>
  <a href="https://www.2xso.com">
  哇塞悲哀的博客
  </a>
</p>
https://www.npmjs.com/package/d_cascader
# d_cascader

 这里是[文档](https://github.com/dongasai/d_cascader#readme)

## Installation

```javascript
npm install d_cascader --save
```
## Usage
**Register component**

Registe global component:

```javascript
import d_cascader from 'd_cascader'

Vue.component('d_cascader', d_cascader)
```

Registe component:

```javascript
import d_cascader from 'd_cascader'

export default {
  components: { d_cascader }
}
```

**How to use**

Basic:

```html
<d_cascader :cdate="citylist" > 
</d_cascader>
```
```javascript
var     citylist=[
  {
    id:"0",
    name:"名字-0"，
    unid："0"
  },{
    id:"1",
    name:"名字-1"，
    unid："0"
  },
  {
    id:"2",
    name:"名字-2"，
    unid："1"
  }
  ,
  {
    id:"3",
    name:"名字-3"，
    unid："1"
  }
];
```


Default Value:

```html
<v-distpicker :cdate="citylist" :de_value="devalue">
</v-distpicker>
```
```javascript
var     citylist=[
  {
    id:"0",
    name:"名字-0"，
    unid："0"
  },{
    id:"1",
    name:"名字-1"，
    unid："0"
  },
  {
    id:"2",
    name:"名字-2"，
    unid："1"
  }
  ,
  {
    id:"3",
    name:"名字-3"，
    unid："1"
  }
];

var de_value=[
  0,3
];
```