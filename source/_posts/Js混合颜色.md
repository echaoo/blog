---
title: Js混合颜色
date: 2017-09-02 19：03
tags: web前端
categories: notes
---
最近需要用js来计算颜色混合后的值，在网上查到了一些可以直接进行计算的公式，但是试验后发现，这些公式只在一部分情况下可以混合出我们想要的颜色(对于颜色我不懂), 经过搜集和实验，找到了一种可靠的方法，记录如下：

###### demo地址: https://echaoo.github.io/demos/color

```javascript
        
    function toCymk (color) {
            let cyan = 255 - color._rgba[0];
            let magenta = 255 - color._rgba[1];
            let yellow = 255 - color._rgba[2];
            let black = Math.min(cyan, magenta, yellow)
            if (black !== 255) {
                cyan = ((cyan - black) / (255 - black))
                magenta = ((magenta - black) / (255 - black))
                yellow = ((yellow - black) / (255 - black))
            } else {
                cyan = 0
                magenta = 0
                yellow = 0
            }
            return {c: cyan, m: magenta, y: yellow, k: black / 255, a: 1}
          }
     function toRgba (color){
            let R = color.c * (1.0 - color.k) + color.k
            let G = color.m * (1.0 - color.k) + color.k
            let B = color.y * (1.0 - color.k) + color.k
            R = Math.round((1.0 - R) * 255.0 + 0.5)
            G = Math.round((1.0 - G) * 255.0 + 0.5)
            B = Math.round((1.0 - B) * 255.0 + 0.5)
            let fcolor = {_rgba: [R, G, B, color.a]}
            return fcolor
          }
    function mix (color1){
            // if(typeof(color1) === 'object'&&(color1 instanceof Array) === false)
            //   color1 = new Array(color1,color2)
    
            let C = 0
            let M = 0
            let Y = 0
            let K = 0
            let A = 0
            for (var i = 0; i < color1.length; i++) {
              color1[i] = toCymk(color1[i])
              console.log(color1[i])
              C += color1[i].c
              M += color1[i].m
              Y += color1[i].y
              K += color1[i].k
              A += color1[i].a
            }
            C = C / color1.length
            M = M / color1.length
            Y = Y / color1.length
            K = K / color1.length
            A = A / color1.length
            let color = {c: C, m: M, y: Y, k: K, a: A}
            let fcolor = toRgba(color)
            return fcolor
          }
    function convertColor (color) {
            let colorstr = color.slice(1, 7)
            let cyan = parseInt(colorstr.slice(0, 2), 16)
            let magenta = parseInt(colorstr.slice(2, 4), 16)
            let yellow = parseInt(colorstr.slice(4, 6), 16)
            return {_rgba: [cyan, magenta, yellow, 1]}
          }
    function toHexString (colorLists) {
            let colorList = colorLists.map(function(i){return convertColor(i)})
            let fanalColor = colorList[0]
            for (let j = 1; j < colorList.length; j++) {
                let midColor = mix([fanalColor, colorList[j]])._rgba
                fanalColor = '#' + midColor[0].toString(16) + midColor[1].toString(16) + midColor[2].toString(16)
            }
            return fanalColor
        }

```

这是主要用到的方法，示例用法如下:
```javascript
let colorList = ['#faffaf', '#059060']
toHexString(colorList)
```
以上

