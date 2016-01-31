
Cocos2d Tutorial

### Monopro 따라하기


``
cocos new 프로젝트명 
``

위와 같이 command 를 작성하면 cocos측에서 기본적으로 실행이 가능한 파일구조를 작성해준다.
이 중에서 res 폴더에는 그림파일, 음악파일 등의 자료가 위치하고, src 폴더의 resource.js에서
변수로서 작성해둔다.

```javascript
var res = {
    pngBgStar  : "res/HappyBird/bg_star.png"
  , pngBird    : "res/HappyBird/bird.png"
  , pngBlock   : "res/HappyBird/block.png"
  , pngGamover : "res/HappyBird/gameoverIMG.png"
  , mp3Bgm   : "res/HappyBird/bgm.mp3"
  , mp3Hit   : "res/HappyBird/hit_sound.mp3"
  , mp3Swing : "res/HappyBird/swing.mp3"
};
```

app.js 에는 게임 화면에 대한 내용이 작성되어있다.
MonoproScene 이라는 화면정보를 작성, 내부에서는 MonoproLayer 라는 
한 레이어 정보를 호출하고 있다.

init 으로 작성하지않고 ctor 로 작성하면 바로 실행되는데 무슨 차이일까?

```javascript
var MonoproScene = cc.Scene.extend({
    onEnter:function () {
        this._super();

        // 내부 init() 메소드 수행
        var layer = new MonoproLayer();
        layer.init();
        
        this.addChild(layer);
    }
});
```

Sprite 는 하나의 캐릭터 단위로 생각하면 될듯.
new 를 이용해서 작성하고 (cocos2d 3.x대 버젼이라)
인자값으로 res 의 png 파일을 지정한다. 
-> 물론, 전역변수 만빵인 지금 상태는 언젠가 수정되어야 할것이다.

```
this.sprite.runAction(
    // 1), 2), 3) 을 순서대로 시행
    cc.sequence(
        // 1) 0.01초 4배로 크게 만듬
        cc.scaleTo(0.01, 4, 4)
        // 2), 3) 을 동시에 시행
      , cc.spawn(
            // 2) 0.3초 360도 회전
            cc.rotateBy(0.3, 360)
            // 3) 0.3초 원래 크기로 돌림 
          , cc.scaleTo(0.3, 1, 1)
        )
    )
);

```

캐릭터에 대한 액션은 위와같은 방법으로 수행한다.
