# InvaderZed
[Live Link](https://onepunchmanz.github.io/InvaderZed/)

InvaderZed is a Galaga inspired game, battle the never ending hoard of space invaders! 

## Table of Contents
- [Technology Stack](#Technology-Stack)
- [How to Use](#How-to-Use)
- [Features](#Features)
- [Future Features](#Future-Features)


## Technology Stack

InvaderZed is created using Vanilla JS, and running Canvas! No fancy engines here. 

## How to Use 

Simply use the arrow keys to navigate, and the space bar to blast your laser at the enemies! 

## Features

<img src="https://github.com/OnePunchManz/JS-Game/blob/master/invaderZed.gif" width="95%" align="center" > 

Custom Pool object. Holds Bullet objects to be managed to prevent garbage collection. When the pool is initialized, it popoulates an array with Bullet objects.W hen the pool needs to create a new object for use, it looks atthe last item in the array and checks to see if it is currently in use or not. If it is in use, the pool is full. If it is not in use, the pool "spawns" the last item in the array and then pops it from the end and pushed it back onto the front of the array. This makes the pool have free objects on the back and used objects in the front. When the pool animates its objects, it checks to see if the object is in use (no need to draw unused objects) and if it is, draws it. If the draw() function returns true, the object is ready to be cleaned so it "clears" the object and uses the array function splice() to remove the item from the array and pushes it to the back. Doing this makes creating/destroying objects in the pool constant.

```javascript
function Pool(maxSize) {
    var size = maxSize; // Max bullets allowed in the pool
    var pool = [];

    this.getPool = function () {
        var obj = [];
        for (var i = 0; i < size; i++) {
            if (pool[i].alive) {
                obj.push(pool[i]);
            }
        }
        return obj;
    }

	/*
	 * Populates the pool array with the given object
	 */
    this.init = function (object) {
        if (object == "bullet") {
            for (var i = 0; i < size; i++) {
                // Initalize the object
                var bullet = new Bullet("bullet");
                bullet.init(0, 0, imageRepository.bullet.width,
                    imageRepository.bullet.height);
                bullet.collidableWith = "enemy";
                bullet.type = "bullet";
                pool[i] = bullet;
            }
        }
        else if (object == "enemy") {
            for (var i = 0; i < size; i++) {
                var enemy = new Enemy();
                enemy.init(0, 0, imageRepository.enemy.width,
                    imageRepository.enemy.height);
                pool[i] = enemy;
            }
        }
        else if (object == "enemyBullet") {
            for (var i = 0; i < size; i++) {
                var bullet = new Bullet("enemyBullet");
                bullet.init(0, 0, imageRepository.enemyBullet.width,
                    imageRepository.enemyBullet.height);
                bullet.collidableWith = "ship";
                bullet.type = "enemyBullet";
                pool[i] = bullet;
            }
        }
    };
```

If the bullet moves off the screen, a new bullet is drawn. 

```javascript
this.draw = function () {
        this.context.clearRect(this.x - 1, this.y - 1, this.width + 2, this.height + 2);
        this.y -= this.speed;

        if (this.isColliding) {
            return true;
        }
        else if (self === "bullet" && this.y <= 0 - this.height) {
            return true;
        }
        else if (self === "enemyBullet" && this.y >= this.canvasHeight) {
            return true;
        }
        else {
            if (self === "bullet") {
                this.context.drawImage(imageRepository.bullet, this.x, this.y);
            }
            else if (self === "enemyBullet") {
                this.context.drawImage(imageRepository.enemyBullet, this.x, this.y);
            }

            return false;
        }
    }
```

Collision detection: 

```javascript
function detectCollision() {
    var objects = [];
    game.quadTree.getAllObjects(objects);

    for (var x = 0, len = objects.length; x < len; x++) {
        game.quadTree.findObjects(obj = [], objects[x]);

        for (y = 0, length = obj.length; y < length; y++) {

            // DETECT COLLISION ALGORITHM
            if (objects[x].collidableWith === obj[y].type &&
                (objects[x].x < obj[y].x + obj[y].width &&
                    objects[x].x + objects[x].width > obj[y].x &&
                    objects[x].y < obj[y].y + obj[y].height &&
                    objects[x].y + objects[x].height > obj[y].y)) {
                objects[x].isColliding = true;
                obj[y].isColliding = true;
            }
        }
    }
};
```



 ## Future Features
   
   1. Boss Battles
   2. Multiplayer
    
  
    
