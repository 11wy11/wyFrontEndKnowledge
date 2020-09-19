## 实现 flatten 函数
```js
let arr = [1, 2, [3, 4, 5, [6, 7], 8], 9, 10, [11, [12, 13]]]
```

### 迭代版
每次拿数组的第一个元素，当判断第一个元素是数组的时候，使用`arrs.unshift(...item)`来
```js
//递归版
let arr = [1, 2, [3, 4, 5, [6, 7], 8], 9, 10, [11, [12, 13]]]
function flatter(arr){
    var newArray=[]
    if(arr.length<1){
        return newArray;
    }else{
        _flatter(newArray,arr);
    }
    return newArray;
    function _flatter(result,arr){
        for(let item of arr){
            if(Array.isArray(item)){
                 _flatter(result,item)
            }else{
                result.push(item)
            }
        }
    }
}
//高级版
function flatter1(arr) {
    while (arr.some(item => Array.isArray(item))) {
      arr = [].concat(...arr)
    }
    return arr
  }

  //迭代版
  function flatter2(arr) {
    let arrs =[...arr]
    let newArr = [];
    while (arrs.length) {
        let item = arrs.shift()
        if(Array.isArray(item)){
            arrs.unshift(...item)   
        }else {
            newArr.push(item)
        }
    }
    return newArr
}

/**
 * 根据参数决定展开深度，默认全部展开
 */
function flattenWithDeep(arr,deep=Infinity){
    let _arr=[...arr];
    let res =[];
    for(let i = 0;i<_arr.length;i++){
        let item=_arr[i];
        if(Array.isArray(item)&&deep>0){
           res=[...res,...flattenWithDeep(item,deep-1)];
        }else{
           res.push(item);
        }
    }
    return res;
}
```