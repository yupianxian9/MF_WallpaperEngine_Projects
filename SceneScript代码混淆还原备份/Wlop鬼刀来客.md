```js
'use strict';

shared = {
    ts: true
};
var timer = 0;
var alphaValue = 1;  // 控制透明度的变量

export function update(value) {
    // 增加计时器
    timer += engine.frametime;
  
    if (shared.ts) {
        // 如果图层存在，控制可见性和透明度
        if (thisScene.getLayer('开场动画')) {
            thisScene.getLayer('开场动画').visible = true;
            thisScene.getLayer('开场动画').alpha = alphaValue;
            thisScene.getLayer('logo').visible = true;
            thisScene.getLayer('logo').alpha = alphaValue;

            // 如果计时器达到5秒，开始淡出
            if (timer >= 3) {
                alphaValue -= engine.frametime * 2; // 控制透明度渐变速度
                if (alphaValue <= 0) {
                    alphaValue = 0; // 确保透明度不会小于0
                    thisScene.getLayer('logo').visible = false;
                    thisScene.getLayer('开场动画').visible = false;
  
                    // 在图层完全不可见时销毁图层
                    thisScene.destroyLayer('logo');
                    thisScene.destroyLayer('开场动画');
                }
            }
        }
    }
    return value;
}

  

export function applyUserProperties(changedUserProperties) {
    if (changedUserProperties.hasOwnProperty('newproperty2')) {
        shared.ts = changedUserProperties.newproperty2;

        // 如果 newproperty39 为 false，立即销毁 '提示' 图层
        if (!shared.ts && thisScene.getLayer('logo')) {
            thisScene.destroyLayer('logo');
        }
        if (!shared.ts && thisScene.getLayer('开场动画')) {
            thisScene.destroyLayer('开场动画');
        }

    }

}
```