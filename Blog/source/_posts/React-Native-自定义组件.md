---
title: React Native 自定义组件
---
### ES6语法

**定义组件**
在ES6里，我们通过定义一个继承自React.Component的class来定义一个组件类，像这样：

```js
//ES6
1. class Photo extends React.Component {
2.     render() {
3.         return (
4.             <Image source={this.props.source} />
5.         );
6.     }
7. }

```
**定义组件的属性类型和默认属性**


在ES6里，可以统一使用static成员来实现

```js
//ES6
1. class Video extends React.Component {
2.     static defaultProps = {
3.         autoPlay: false,
4.         maxLoops: 10,
5.     };  // 注意这里有分号
6.     static propTypes = {
7.         autoPlay: React.PropTypes.bool.isRequired,
8.         maxLoops: React.PropTypes.number.isRequired,
9.         posterFrameSrc: React.PropTypes.string.isRequired,
1.         videoSrc: React.PropTypes.string.isRequired,
2.     };  // 注意这里有分号
3.     render() {
4.         return (
5.             <View />
6.         );
7.     } // 注意这里既没有分号也没有逗号
8. }

```


-------

#### 正文

首先

```js
import React, { Component ,PropTypes} from 'react';
```

必须包含PropTypes，这是为了规范组件属性的数据类型

```js
import {
   Text,
   View,
   TouchableOpacity,
 } from 'react-native';
```

设置默认属性

```js
1. static defaultProps = {
2.      defaultColor:'#f44e14',
3.      buttonName:'Button',
4.    }
```
设置对外接收的属性，以及属性的数据类型

```js
1.  static propTypes = {
2.      setBackgroundColor : PropTypes.string,
3.      buttonName : PropTypes.string,
4.      block : PropTypes.func,
5.      setWidth : PropTypes.number,
6.      setHeight : PropTypes.number,
7.    }
```
上面的block属性我设置的是一个func类型，也就是函数，在这里就是起一个回调作用。

```
1. render() {
2.      return(
3.        <TouchableOpacity onPress={()=>this.props.block()} >
4.          <View style={{
5.           flexDirection : 'row',
6.          justifyContent : 'center',
7.              alignItems : 'center',
8.         backgroundColor : (this.props.setBackgroundColor) ? this.props.setBackgroundColor : this.props.defaultColor,
9.                   width : (this.props.setWidth!==0) ? this.props.setWidth : 60 ,
1.                  height : (this.props.setHeight!==0) ? this.props.setHeight : 20,}}
2.          >
3.            <Text>{this.props.buttonName}</Text>
4.          </View>
5.        </TouchableOpacity>
6.      );
7.    }
```
#### 外部使用

引入自定义组件文件

```
<TouchableOpacity onPress={()=>this.props.block()} >//执行外部传进来的函数
```
#### 自定义组件完整代码
```js
1. import React, { Component ,PropTypes} from 'react';
2.  import {
3.    Text,
4.    View,
5.    TouchableOpacity,
6.  } from 'react-native';
7.  class DiscolorationButton extends React.Component{
8.    static defaultProps = {
9.      defaultColor:'#f44e14',
1.      buttonName:'Button',
2.    }
3.    static propTypes = {
4.      setBackgroundColor : PropTypes.string,
5.      buttonName : PropTypes.string,
6.      block : PropTypes.func,
7.      setWidth : PropTypes.number,
8.      setHeight : PropTypes.number,
9.    }
1.    render() {
2.      return(
3.        <TouchableOpacity onPress={()=>this.props.block()} >
4.          <View style={{
5.           flexDirection : 'row',
6.          justifyContent : 'center',
7.              alignItems : 'center',
8.         backgroundColor : (this.props.setBackgroundColor) ? this.props.setBackgroundColor : this.props.defaultColor,
9.                   width : (this.props.setWidth!==0) ? this.props.setWidth : 60 ,
1.                  height : (this.props.setHeight!==0) ? this.props.setHeight : 20,}}
2.          >
3.            <Text>{this.props.buttonName}</Text>
4.          </View>
5.        </TouchableOpacity>
6.      );
7.    }
8.  }
9. module.exports = DiscolorationButton;
```

<div align='center'\>
![效果图](http://upload-images.jianshu.io/upload_images/2067180-e0547192bd5c6831.gif?imageMogr2/auto-orient/strip)


### 外部调用
```js
1. import React, { Component } from 'react';
2. import {
3.   AppRegistry,
4.   StyleSheet,
5.   Text,
6.   View
7. } from 'react-native';
8. import DiscolorationButton from './DiscolorationButton.js'
9. export default class test extends Component {
1.   test(){
2.     alert('我是测试函数');
3.   }
4.   render() {
5.     return (
6.       <View style={styles.container}>
7.         <DiscolorationButton

8.         setBackgroundColor = '#bfb'
9.                 buttonName = '点我'
1.                   setWidth = {80}
2.                  setHeight = {40}
3.                      block = {()=>this.test()}
4.          />
5.       </View>
6.     );
7.   }
8. }

9. const styles = StyleSheet.create({
1.   container: {
2.     flex: 1,
3.     justifyContent: 'center',
4.     alignItems: 'center',
5.     backgroundColor: '#F5FCFF',
6.   },
7. });
8. AppRegistry.registerComponent('test', () => test);
```


