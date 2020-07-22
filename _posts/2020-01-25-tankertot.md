---
title: "tankerTot"
date: 2020-01-25
tags: [Video Game]
excerpt: "Using a Javascript code-based game engine, Phaser 3, my team and I created a fun and challenging web game that went on to win a local award."
---
## Introduction
This project started late August 2019 as part of a semester project for CS 329E
Elements of Game Development at The University of Texas at Austin. We formed in teams of
three tasked with creating a web-based video game from scratch by using the Phaser
3 game engine which we all had no experience with prior to the course.

We originally had to pitch an idea to the professor and TA, so after brainstorming
and original rejection of a top-down dungeon crawler we settled on a physics based
puzzle shooter game designed around a tank riding Corgi went to battle with squirrels.
We got the idea after the professor's love of tanks and the TA's pet Corgi taterTot,
thus we named the game tankerTot.

tankerTot is available to be played online at: [tankerTot](https://basilio0505.itch.io/tankertot)
The Repository can be found here: [github](https://github.com/Basilio0505/tankerTotGame)

This project ended up winning the Excellence in Creativity Award from the Longhorn
Creators Foundation and my team and I received a grant for our work.

Here is a gameplay trailer of the game:
<iframe width="560" height="315" src="https://www.youtube.com/embed/zUASnX3n0Go" frameborder="0" allowfullscreen></iframe>

## Tools
The main tool used for this project was Phaser 3, a code-based game engine used to
create web flash games with HTML and Javascript. Going into this project I had no
prior experience to using this engine with some introduction tutorial videos and referring
to the documentation. It was a challenging obstacle to overcome but my team and I
ended up creating a great game.

Since all art assets had to be made from scratch we used various free art tools like
Microsoft Paint3D for the tutorial text panels and environment art, Microsoft Paint
for all in-game UI elements and an online pixel art creator for all sprites.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/tankertot/tankerTot-Level2.jpg" alt="tankerTot Level 2">

## My Role
I had more of an engineer role on this project with some art as well. Being a physics-based
puzzle-shooter, I was in charge of getting the mechanics of the game to work. More
specifically I constructed the bullet bounce physics where the player could fire
bullets that would bounce off walls and have durability. I also worked on collision
detection between different types of objects and the goal/level progression of the
game.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/tankertot/tankerTot-Win.jpg" alt="tankerTot Win Screen">

I also help create the tutorial section, creating the art for the text panels of
instruction.

Here is a code snippet of how collision detection works between different objects:
```javascript
//Decects collision of two objects
    this.matter.world.on('collisionstart', function(event){
      if (event.pairs[0].bodyB.gameObject == this.bullet){
        //Checks if the two objects colliding are the regular squirrel and bullet
        if(event.pairs[0].bodyA.gameObject == squirrel){
          squirrel.destroy();
          this.squirrelCount -= 1;
          this.sound.play('squirreldeath');
        }
        //Checks if the two objects colliding are the player and the player bullet
        else if(event.pairs[0].bodyA.gameObject == this.player){
          //GAME OVER
          this.registry.set('selfHit', true);
          this.registry.set('Level7Score', 0);
          if(this.registry.get('Level7HighScore') < this.registry.get('Level7Score')){
            this.registry.set('Level7HighScore', this.registry.get('Level7Score'));
          }
          this.scene.start('Section3End', {
            backgroundX: this.background.tilePositionX,
            buildingsfX: this.buildingsf.tilePositionX,
            buildingsbX: this.buildingsb.tilePositionX,
            tankerX: this.player.x
            });
        }
        //Checks if the two objects colliding are the walls or platforms and bullet
        else if(event.pairs[0].bodyA.gameObject == this.plat1 { //|| OR ALL OTHER PLATFORMS
          this.bounceCount += 1;
          if(this.bounceCount % 2 == 0){
            this.bullet.setFrame(this.bounceCount/2);
          }
          this.sound.play('bounce');
        }
      }  
```
