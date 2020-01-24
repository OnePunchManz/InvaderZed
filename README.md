# InvaderZed
[Live Link](https://onepunchmanz.github.io/JS-Game/)

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

 ## Future Features
   
   1. Boss Battles
   2. Multiplayer
    
  
    
