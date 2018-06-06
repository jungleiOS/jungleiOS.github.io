---
title: ReactNative ListView轻松上手
date: 2017.04.23 18:25
---
感觉自己需要提一下了，最近简单的写了个demo练练手，主要记录下自己的收获。

在移动开发中我们用得最多和最重要的一个控件就是滚动视图，在iOS开发中我们用的这个控件叫做TableView，在Android开发中这个控件叫做ListView，然后就是现在的跨平台开发。最近最火的跨平台开发框架当属Facebook开源的React-Native了。在React-Native中也有一个实现滚动视图的组件也叫做ListView

最近在工作中接触到了这个开源框架，在项目中用到了RN（React-Natove简写）的ListView，然后自己写了个简单的demo。

RN中ListView的属性就不一一讲解了，在ReactNative中文中已经讲得很详细了。
不管是RN中的ListView还是iOS中的TableView都得有数据源的即DataSource，因为RN跨平台框架底层是调用原生的控件。
<div align='center'\>
![](https://upload-images.jianshu.io/upload_images/2067180-2a1488c0631620ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/2067180-690e114a045bfacd.gif?imageMogr2/auto-orient/strip)
那么在RN中是这样设置数据源的

创建一个ListView.DataSource类型的对象this.ds

```js
this.ds = new ListView.DataSource({rowHasChanged:(r1,r2)=>r1!==r2})
```

创建一个数组this.datasource并赋值，这个数组中装着的是5个对象。

```js
this.dataSource = [  {name:'柴小圣',id:1,picCategory:'写真图',picUri:'http://res.mamiw.com/uploads/allimg/160408/0450044201-0.jpg',picDescribe:'极品美女柴小圣美艳写真图',messageNumber:99,label:['搞笑六点半','网剧'],firstLabelImage:'https://tse4-mm.cn.bing.net/th?id=OIP.ngQ_96uWK-a50WUtnjGagAEsEP&w=221&h=200&c=7&qlt=90&o=4&pid=1.7'},
                         {name:'柴小圣',id:2,picCategory:'写真图',picUri:'http://res.mamiw.com/uploads/allimg/160408/0450045C3-1.jpg',picDescribe:'O(∩_∩)O哈哈~美艳写真图',messageNumber:43,label:['欢乐者联盟','网剧','哈哈'],firstLabelImage:'https://tse4-mm.cn.bing.net/th?id=OIP.wsbd4ipN95lWxm-6g5aINQEsEs&w=200&h=200&c=7&qlt=90&o=4&pid=1.7'},
                         {name:'柴小圣',id:3,picCategory:'写真图',picUri:'http://res.mamiw.com/uploads/allimg/160408/0450042929-2.jpg',picDescribe:'(～￣▽￣)～漂亮妹纸',messageNumber:55,label:['搞笑'],firstLabelImage:'https://tse4-mm.cn.bing.net/th?q=杰尼龟&w=120&h=120&c=1&rs=1&qlt=90&pid=InlineBlock&mkt=zh-CN&adlt=strict&t=1&mw=247'},
                         {name:'柴小圣',id:4,picCategory:'写真图',picUri:'http://res.mamiw.com/uploads/allimg/160408/0450041D6-3.jpg',picDescribe:'看看✧(≖ ◡ ≖✿)嘿嘿',messageNumber:33,label:['奇葩使者','雷剧','Ls'],firstLabelImage:'https://tse3-mm.cn.bing.net/th?q=可达鸭&w=120&h=120&c=1&rs=1&qlt=90&pid=InlineBlock&mkt=zh-CN&adlt=strict&t=1&mw=247'},
                         {name:'柴小圣',id:5,picCategory:'写真图',picUri:'http://res.mamiw.com/uploads/allimg/160408/045004O27-4.jpg',picDescribe:'极品美女柴小圣美艳写真图',messageNumber:99,label:['搞笑六点半','牛'],firstLabelImage:'https://tse2-mm.cn.bing.net/th?q=加菲猫&w=120&h=120&c=1&rs=1&qlt=90&pid=InlineBlock&mkt=zh-CN&adlt=strict&t=1&mw=247'},
                       ]
```

使用this.ds.cloneWithRows方法为ListViewDataSource复制填充数据this.dataSource

```js
this.state = {
      dataSource:this.ds.cloneWithRows(this.dataSource)
    }
```
使用ListView组件

```js
//设置数据源和每行的实际渲染
<ListView dataSource={this.state.dataSource} renderRow={this._renderRow}/>
```

ListView组件的renderRow属性

> renderRow function 

> (rowData, sectionID, rowID, highlightRow) => renderable

> 从数据源(Data source)中接受一条数据，以及它和它所在section的ID。返回一个可渲染的组件来为这行数据进行渲染。默认情况下参数中的数据就是放进数据源中的数据本身，不过也可以提供一些转换器。

> 如果某一行正在被高亮（通过调用highlightRow函数），ListView会得到相应的通知。当一行被高亮时，其两侧的分割线会被隐藏。行的高亮状态可以通过调用highlightRow(null)来重置。

这里的renderRow使用起来有点类似iOS开发中的自定义Cell

具体使用

```js
  _renderRow(rowData: string, sectionID: number, rowID: number){
    return(
      <View key={rowID}>
        <ImageTextMixedConfigurationCell imageUri={rowData.picUri} text = {rowData.picDescribe}/>
        <View style={styles.bottomStyle}>
          <TEMlabelButton totalWidth = {1/2*WIDTH}
                          imageUri = {rowData.firstLabelImage}
                          selected = {(obj)=>{alert('text='+obj.labelText+'selectedIndex='+obj.selectedIndex)}}
                          id = {rowData.id}
                          titleArray = {rowData.label}/>
          <BottomRightView messageNumber = {rowData.messageNumber} rowID = {rowID}/>
        </View>
      </View>
    )
  }
```
ListView组件使用完整代码

```js
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Dimensions,
  TouchableOpacity,
  ListView,
  Image,
  Text,
  View
} from 'react-native';
/**
 *引入自定义组件
 */
import ImageTextMixedConfigurationCell from './ImageTextMixedConfigurationCell.js';
import TEMcollectionButton from './CollectionButton.js';
import BottomRightView from './BottomRightView.js';
import TEMlabelButton from './LabelButton.js';
/**
 *关闭烦人的警告框（关闭有好有坏自行斟酌）
 */
console.disableYellowBox = true;
/**
 *引入系统监听机制
 */
import RCTDeviceEventEmitter from 'RCTDeviceEventEmitter';

export const WIDTH = Dimensions.get('window').width;
export const HEIGHT = Dimensions.get('window').height;

export default class ListViewDemo extends Component {
  constructor(props){
    super(props)
    this.ds = new ListView.DataSource({rowHasChanged:(r1,r2)=>r1!==r2})
    this.dataSource = [  {name:'柴小圣',id:1,picCategory:'写真图',picUri:'http://res.mamiw.com/uploads/allimg/160408/0450044201-0.jpg',picDescribe:'极品美女柴小圣美艳写真图',messageNumber:99,label:['搞笑六点半','网剧'],firstLabelImage:'https://tse4-mm.cn.bing.net/th?id=OIP.ngQ_96uWK-a50WUtnjGagAEsEP&w=221&h=200&c=7&qlt=90&o=4&pid=1.7'},
                         {name:'柴小圣',id:2,picCategory:'写真图',picUri:'http://res.mamiw.com/uploads/allimg/160408/0450045C3-1.jpg',picDescribe:'O(∩_∩)O哈哈~美艳写真图',messageNumber:43,label:['欢乐者联盟','网剧','哈哈'],firstLabelImage:'https://tse4-mm.cn.bing.net/th?id=OIP.wsbd4ipN95lWxm-6g5aINQEsEs&w=200&h=200&c=7&qlt=90&o=4&pid=1.7'},
                         {name:'柴小圣',id:3,picCategory:'写真图',picUri:'http://res.mamiw.com/uploads/allimg/160408/0450042929-2.jpg',picDescribe:'(～￣▽￣)～漂亮妹纸',messageNumber:55,label:['搞笑'],firstLabelImage:'https://tse4-mm.cn.bing.net/th?q=杰尼龟&w=120&h=120&c=1&rs=1&qlt=90&pid=InlineBlock&mkt=zh-CN&adlt=strict&t=1&mw=247'},
                         {name:'柴小圣',id:4,picCategory:'写真图',picUri:'http://res.mamiw.com/uploads/allimg/160408/0450041D6-3.jpg',picDescribe:'看看✧(≖ ◡ ≖✿)嘿嘿',messageNumber:33,label:['奇葩使者','雷剧','Ls'],firstLabelImage:'https://tse3-mm.cn.bing.net/th?q=可达鸭&w=120&h=120&c=1&rs=1&qlt=90&pid=InlineBlock&mkt=zh-CN&adlt=strict&t=1&mw=247'},
                         {name:'柴小圣',id:5,picCategory:'写真图',picUri:'http://res.mamiw.com/uploads/allimg/160408/045004O27-4.jpg',picDescribe:'极品美女柴小圣美艳写真图',messageNumber:99,label:['搞笑六点半','牛'],firstLabelImage:'https://tse2-mm.cn.bing.net/th?q=加菲猫&w=120&h=120&c=1&rs=1&qlt=90&pid=InlineBlock&mkt=zh-CN&adlt=strict&t=1&mw=247'},
                       ]
    this.state = {
      dataSource:this.ds.cloneWithRows(this.dataSource)
    }
  }
  componentDidMount(){
    this.listenerA=RCTDeviceEventEmitter.addListener('deletCellEvent',(rowID) => {
      this.deletCell(rowID);
    });
  }
  componentWillUnmount(){
    this.listenerA.remove();
  }
  deletCell(rowID){
    this.dataSource.splice(rowID,1);
    this.setState({
      dataSource : this.ds.cloneWithRows(this.dataSource)
    })
  }
  _renderRow(rowData: string, sectionID: number, rowID: number){
    return(
      <View key={rowID}>
        <ImageTextMixedConfigurationCell imageUri={rowData.picUri} text = {rowData.picDescribe}/>
        <View style={styles.bottomStyle}>
          <TEMlabelButton totalWidth = {1/2*WIDTH}
                          imageUri = {rowData.firstLabelImage}
                          selected = {(obj)=>{alert('text='+obj.labelText+'selectedIndex='+obj.selectedIndex)}}
                          id = {rowData.id}
                          titleArray = {rowData.label}/>
          <BottomRightView messageNumber = {rowData.messageNumber} rowID = {rowID}/>
        </View>
      </View>
    )
  }
  render() {
    return (
      <View style={styles.container}>
        <View style={styles.topBar}/>
        <ListView dataSource={this.state.dataSource} renderRow={this._renderRow}/>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#F5FCFF',
  },
  topBar: {
    width: WIDTH,
    height: 20,
  },
  bottomStyle: {
    flexDirection: 'row',
    alignItems: 'center',
    justifyContent: 'space-between',
    width: WIDTH,
    backgroundColor: '#FFF',
    height: 40
  },
});

AppRegistry.registerComponent('ListViewDemo', () => ListViewDemo);

```

完整demo
https://github.com/jungleiOS/RN_ListView.git
如果大家觉得喜欢请在GitHub上赏作者一个Star吧


