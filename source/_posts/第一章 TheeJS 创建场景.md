---
title: ThreeJS学习 第一章 创建一个场景
catalog: true
tags:
  - HTML
  - JavaScript
  - Threejs
categories:
  - ThreeJS
date: 2019-08-05 16:37:05
subtitle:
header-img: "http://b-ssl.duitang.com/uploads/item/201608/06/20160806232840_WZCjT.jpeg"
---

#### 1.创建基本的页面结构

> 从Threejs官网下载Three.js 到本地并且引入此文件。 [下载地址](https://threejs.org/build/three.js)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Learn Three.js</title>
    <style> *{padding:0;margin:0;}
    body { margin: 0; }
    canvas { width: 100%; height: 100% }
    </style>
</head>
<body>

</body>
<script src="./js/three.js"></script>
<script>
  // TODO
</script>
</html>


```

#### 2. 创建一个简单的场景

> 为了真正能够让你的场景借助three.js来进行显示，我们需要以下几个对象：场景、相机和渲染器，这样我们就能透过摄像机渲染出场景。

```javascript
   // 创建一个场景
    let scene = new THREE.Scene()
    // 创建一个相机
    let camera = new THREE.PerspectiveCamera(75,window.innerWidth/window.innerHeight,0.1, 1000)
    // 创建一个渲染器
    let renderer = new THREE.WebGLRenderer();
    // 设定渲染的区域
    renderer.setSize(window.innerWidth,window.innerHeight)
    // 添加DOM
    document.body.appendChild(renderer.domElement)

```

我们花一点点时间来解释一下这里发生了什么。我们现在建立了场景、相机和渲染器。three.js里有几种不同的相机，在这里，我们使用的是PerspectiveCamera（透视摄像机）。

第一个参数是视野角度（FOV）。视野角度就是无论在什么时候，你所能在显示器上看到的场景的范围，它的值是角度单位。第二个参数是长宽比（aspect ratio）。 也就是你用一个物体的宽除以它的高的值。比如说，当你在一个宽屏电视上播放老电影时，可以看到图像仿佛是被压扁的。

接下来的两个参数是近截面（near）和远截面（far）。 当物体某些部分比摄像机的远截面远或者比近截面近的时候，该这些部分将不会被渲染到场景中。你或许现在并不用担心这个值的影响，但未来为了获得更好的渲染性能，你将可以在你的应用程序里去设置它。

接下来是渲染器。这里是施展魔法的地方。除了我们在这里用到的WebGLRenderer渲染器之外，Three.js同时提供了其他几种渲染器，当用户所使用的浏览器过于老旧，或者由于其他原因不支持WebGL时，可以使用这几种渲染器进行降级。

除了创建一个渲染器的实例之外，我们还需要在我们的应用程序里设置一个渲染器的尺寸。比如说，我们可以使用所需要的渲染区域的宽高，来让渲染器渲染出的场景填充满我们的应用程序。因此，我们可以将渲染器宽高设置为浏览器窗口宽高。对于性能比较敏感的应用程序来说，你可以使用setSize传入一个较小的值，例如window.innerWidth/2和window.innerHeight/2，这将使得应用程序在渲染时，以一半的长宽尺寸渲染场景。

如果你希望保持你的应用程序的尺寸，但是以较低的分辨率来渲染，你可以在调用setSize时，将updateStyle（第三个参数）设为false。例如，假设你的<canvas>标签现在已经具有了100%的宽和高，调用setSize(window.innerWidth/2, window.innerHeight/2, false)将使得你的应用程序以一半的分辨率来进行渲染。

最后一步很重要，我们将renderer（渲染器）的dom元素（renderer.domElement）添加到我们的HTML文档中。这就是渲染器用来显示场景给我们看的<canvas>元素。

#### 3. 创建一个立方体

```javascript
    // 创建一个几何体
    let geometry = new THREE.BoxGeometry(1,1,1)
    // 创建一个材质
    let material = new THREE.MeshBasicMaterial({color: '#FFC107'})
    // 创建一个网格和正方体
    let cube = new THREE.Mesh(geometry,material)
    // 将建立的模型放入场景中
    scene.add( cube )
    // 默认情况下，当我们调用scene.add()的时候，物体将会被添加到(0,0,0)坐标。但将使得摄像机和立方体彼此在一起。为了防止这种情况的发生，我们只需要将摄像机稍微向外移动一些即可。
    camera.position.z = 5

```
要创建一个立方体，我们需要一个BoxGeometry（立方体）对象. 这个对象包含了一个立方体中所有的顶点（vertices）和面（faces）。未来我们将在这方面进行更多的探索。

接下来，对于这个立方体，我们需要给它一个材质，来让它有颜色。Three.js自带了几种材质，在这里我们使用的是MeshBasicMaterial。所有的材质都存有应用于他们的属性的对象。在这里为了简单起见，我们只设置一个color属性，值为#FFC107，也就是橙色。这里所做的事情，和在CSS或者Photoshop中使用十六进制(hex colors)颜色格式来设置颜色的方式一致。

第三步，我们需要一个Mesh（网格）。 网格包含一个几何体以及作用在此几何体上的材质，我们可以直接将网格对象放入到我们的场景中，并让它在场景中自由移动。

默认情况下，当我们调用scene.add()的时候，物体将会被添加到(0,0,0)坐标。但将使得摄像机和立方体彼此在一起。为了防止这种情况的发生，我们只需要将摄像机稍微向外移动一些即可。


#### 4. 渲染创建的场景，使立方体动起来

```javascript
      // 渲染动画
    let animate = function () {
        requestAnimationFrame(animate)
        cube.rotation.x += 0.01
        cube.rotation.y += 0.01
        renderer.render(scene,camera)
    }
    // 调用动画，渲染界面
    animate()

```

现在，如果将之前写好的代码复制到HTML文件中，你不会在页面中看到任何东西。这是因为我们还没有对它进行真正的渲染。为此，我们需要使用“渲染循环”（render loop）或者“动画循环”（animate loop）的东西。

在将创建了一个使渲染器能够在每次屏幕刷新时对场景进行绘制的循环（在大多数屏幕上，刷新率一般是60次/秒）。如果你是一个浏览器游戏开发的新手，你或许会说“为什么我们不直接用setInterval来实现刷新的功能呢？”当然啦，我们的确可以用setInterval，但是，requestAnimationFrame有很多的优点。最重要的一点或许就是当用户切换到其它的标签页时，它会暂停，因此不会浪费用户宝贵的处理器资源，以及损耗电池的使用寿命。

#### 5.结果

![结果图片](https://s2.ax1x.com/2019/08/05/e2WEXF.png)

> 完整代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Learn Three.js</title>
    <style> *{padding:0;margin:0;}
    body { margin: 0; }
    canvas { width: 100%; height: 100% }
    </style>
</head>
<body>

</body>
<script src="./js/three.js"></script>
<script>
    // 创建一个场景
    let scene = new THREE.Scene()
    // 创建一个相机
    let camera = new THREE.PerspectiveCamera(75,window.innerWidth/window.innerHeight,0.1, 1000)
    // 创建一个渲染器
    let renderer = new THREE.WebGLRenderer();
    // 设定渲染的区域
    renderer.setSize(window.innerWidth,window.innerHeight)
    // 添加DOM
    document.body.appendChild(renderer.domElement)
    // 创建一个几何体
    let geometry = new THREE.BoxGeometry(1,1,1,)
    // 创建一个材质
    let material = new THREE.MeshBasicMaterial({color: '#FFC107'})
    // 创建一个网格和正方体
    let cube = new THREE.Mesh(geometry,material)
    // 将建立的模型放入场景中
    scene.add( cube )
    // 默认情况下，当我们调用scene.add()的时候，物体将会被添加到(0,0,0)坐标。但将使得摄像机和立方体彼此在一起。为了防止这种情况的发生，我们只需要将摄像机稍微向外移动一些即可。
    camera.position.z = 5
    // 渲染动画
    let animate = function () {
        requestAnimationFrame(animate)
        cube.rotation.x += 0.01
        cube.rotation.y += 0.01
        renderer.render(scene,camera)
    }
    // 调用动画，渲染界面
    animate()
</script>
</html>

```