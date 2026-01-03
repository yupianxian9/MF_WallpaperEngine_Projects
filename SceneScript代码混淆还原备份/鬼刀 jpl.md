音频识别 透明度
```js
'use strict';
// Please note: Do not remove this line or asset references may break.
/*options start*/
var fade_speed = 1;     //time to fade in (in seconds)
var basic_volume = 1; //basic volume (from 0 to 1)
/*options end*/

var state = 0;
var fade_counter = 0;
fade_speed = 1 / fade_speed;
var ofg = 0
shared.tm = 0
// export function mediaPlaybackChanged(event) {
export function mediaPlaybackChanged(event) {
    
	//state = event.state !== MediaPlaybackEvent.PLAYBACK_PLAYING;
    if(event.state == 0){
		ofg = 0
	}else if(event.state == 1){
		ofg = 1
	}else if(event.state == 2){
		ofg = 2
	}else{
		ofg = 3
	}
    
    // MediaPlaybackEvent.PLAYBACK_PLAYING- 媒体正在系统上积极播放。
    // MediaPlaybackEvent.PLAYBACK_PAUSED- 媒体之前正在播放，但用户（暂时）暂停了播放。
    // MediaPlaybackEvent.PLAYBACK_STOPPED- 媒体播放完全停止。
}
//关闭 0    播放1   暂停2  

export function mediaThumbnailChanged(event) {
    // console.log(event.hasThumbnail)
    if(event.hasThumbnail){
        ofg = 1
    }else{
        ofg = 0
    }
}
var intervalTime = 0
export function update(value) {

    if(ofg == 0){
        fade_counter = fade_counter - (fade_speed * engine.frametime * 2);
        shared.tm = fade_counter
        if (fade_counter < 0) {
            fade_counter = 0;
            intervalTime = 0
            thisScene.getLayer('9999960001').visible = false
        }
	}else if(ofg == 1){
        if(intervalTime <= 3){
            intervalTime += engine.frametime;
            fade_counter = fade_counter + (fade_speed * engine.frametime * 2);
            shared.tm = fade_counter
            thisScene.getLayer('9999960001').visible = true
            if (fade_counter > 1) {
                fade_counter = 1;
                
            }
        }else if(intervalTime >= 3){
            fade_counter = fade_counter - (fade_speed * engine.frametime * 2);
            shared.tm = fade_counter
            if (fade_counter < 0) {
                fade_counter = 0; 
            thisScene.getLayer('9999960001').visible = false
            }
        }
	}else if(ofg == 2){
        fade_counter = fade_counter - (fade_speed * engine.frametime * 2);
        shared.tm = fade_counter
        if (fade_counter < 0) {
            fade_counter = 0;
            intervalTime = 0
            thisScene.getLayer('9999960001').visible = false
        }
	}
    return fade_counter * basic_volume;
}
```

```js
'use strict';
var time = 0
export function update(value) {
	time += engine.frametime
	if(time <= 10){
		thisScene.getLayer('模糊度').visible = true
		thisScene.getLayer('文本3').visible = true
		thisScene.getLayer('纯黑色').visible = true
		thisScene.getLayer('wlop').visible = true
	}else{
		thisScene.getLayer('模糊度').visible = false
		thisScene.getLayer('文本3').visible = false
		thisScene.getLayer('纯黑色').visible = false
		thisScene.getLayer('wlop').visible = false
	}
	return value;
}

```