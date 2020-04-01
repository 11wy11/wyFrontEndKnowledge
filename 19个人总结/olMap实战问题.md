# 改变地图大小

当我们自适应地图大小时，通常采用

 监听window.onresize=function(){

//改变地图大小，会发现有时候会失效，这是需要调用this.nextTick(()=>{this.map.updateSize()})

}

