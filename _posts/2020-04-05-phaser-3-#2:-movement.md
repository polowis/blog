---
date: 2020-04-05 04:13:40
layout: post
title: "Phaser 3 #2: Actions/Movements"
subtitle:
description: Part 2 of phaser series
image: https://phaser.io/images/phaser3/robotune.png
optimized_image:
category: code
tags: 
    - js
    - phaser
    - programming

author: polowis
paginate: false
---

This is part two of the series, if you haven't read the first part yet, you can find it [here](https://polowis.netlify.com/phaser-3-let's-build-a-simple-shooting-game-1/)

Hopefully, you get the idea of how Phaser works so far. In this post, we'll move to the next step. Movement.

## The player actions

The game will be bored to play if you can't control the character. And if you can't control the character, it isn't a game anymore, it's an animation where you watch the sprites move in their own ways and acts as how it is programmed to do so. 

In your ```preload()```function, make sure you add something that represents you in the game. It can be something like this:
```js
preload() {
    this.load.image('player', 'assets/player.png')
}
```
You can also load the player as a spritesheet so that you can use the animation

```js
preload(){
    this.load.spritesheet('player', 'assets/player.png', {frameWidth: 32, frameHeight: 32})
}
```
## The player

Before you add the player like you usually do, let me show you how to do it in a more advanced way. 

create a new file and let's call it ```player.js``` for now. If you are using the NPM version, you will need to import Phaser

```js
import Phaser from 'phaser'
export default class Player extends Phaser.GameObjects.Sprite{

}
```
The export default statement tells the computer to export the given class so that we can use it in other files. In case if you're using CDN version, just remove the export default and import statement. But make sure to include the file in your HTML file.

```js
class Player extends Phaser.GameObjects.Sprite{
}
```
Okay, how are we going to use this? Just like what you do with ```add.image()``` function. 
```js
class Player extends Phaser.GameObjects.Sprite{
    constructor(scene, x, y){
        super(scene, x, y, 'player')
        this.scene = scene
        this.scene.add.existing(this)
        this.scene.physics.world.enableBody(this, 0)
    }
}
```
The ```super()``` keyword is used to access the an object's parent. In our case is ```Phaser.GameObjects.Sprite```, it takes 4 parameters. The first parameter is the current scene, the second parameter is the x coordinates, follow by the y coordinates as the third parameter. And the last one is the sprite name, the name that we delcared in our ```preload()``` function. 

Previously, we added physics to our sprite by adding the word ```physics``` before ```add.sprite()``` function. (ie ```this.physics.add.sprite('player')```). But before we cannot use it in our class, we will need another way to enable physics. The function ```this.scene.physics.world.enableBody(this, 0)``` will enable physics, where ```this``` is our current object which is ```Player```

If you need to scale the image to fit your game, you can just do it like how you do it normally. (ie ```this.scaleX =  1```).

In your ```create()``` function, we need to call the class.

```js
create(){
    this.player = new Player(this, 200, 300)
}
```

The ```this``` keyword in this case refers to the current scene. The [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) docs covers the ```this``` keyword very well.



### Keyboard movement

Implement keyboard movement in Phaser is quite simple, in your ```update``` function, you can add the following function to detect key
```js
this.cursors = this.input.keyboard.createCursorKeys();
```

Back to your ```player.js``` file, add the following function to make your sprite moves
```js
export default class Player extends Phaser.GameObjects.Sprite{
    constructor(){
        super()
    }

    /**
     * 
     * Move the player left
     */
    moveLeft(){
        this.body.velocity.x = - 200;
    }

    /**
     * 
     * Move the player right
     */
    moveRight(){
        this.body.velocity.x = 200;
    }

    moveUp(){
        this.body.velocity.y = - 200;
    }

    moveDown(){
        this.body.velocity.y = 200);
    }
}
```
We will use the ```velocity``` keyword to determine how fast the object is moving upward or downward. I already covered this in the previous [post](https://polowis.netlify.com/phaser-3-let's-build-a-simple-shooting-game-1/).

In our ```update()``` function.
Make sure to add the following functions before any keyboard movement. 
```js
update() {
    this.player.body.velocity.x = 0;
    this.player.body.velocity.y = 0;
}
```
When we press the key, we set the velocity to ```200`` but what happens when we release the key?, the velocity stays the same, therefore we need to fix this. We need to set the velocity of the player to 0 when the inputs stop. Then just add simple if-else statement

```js
    this.cursors = this.input.keyboard.createCursorKeys();

    if (this.cursors.left.isDown) {
        this.player.moveLeft()
    } else if (this.cursors.right.isDown) {
        this.player.moveRight()
    }

    if (this.cursors.up.isDown) {
        this.player.moveUp()
    } else if (this.cursors.down.isDown) {
        this.player.moveDown()
    } 
```

Now if you use your arrow keys, you should be able to see the player move up and down or left and right. 
