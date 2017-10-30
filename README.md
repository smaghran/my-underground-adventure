<!doctype html>
<html>

<head>
    <meta charset="UTF-8" />
    <title>ChickTech JavaScript</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/phaser/2.6.2/phaser.min.js"></script>

    <!--loading our CSS file-->
    <link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
<body>

<script type="text/javascript">
    var game = new Phaser.Game(800, 600, Phaser.AUTO, '', { preload: preload, create: create, update: update });
    var score = 0;
    var scoreText;
    //These three functions are required for phaser!
    //Preload loads everything your game needs - sprites, images, etc
    function preload() {
        game.load.image('sky', 'assets/6.jpg');
        game.load.image('ground', 'assets/platform.png');
        game.load.image("gems", "assets/diamond.png");
        game.load.spritesheet('hp', 'assets/skeleton2.png', 44,49);
        game.load.image("sword", "assets/sword1.png");
    }
    var platforms;
    //Create sets up your game for you - most of your code will probably be here!
    function create() {
        //  We're going to be using physics, so enable the Arcade Physics system
        game.physics.startSystem(Phaser.Physics.ARCADE);
        //add the sky
        game.add.sprite(0, 0, 'sky');
        //add a group to hold the ground
        platforms = game.add.group();
        //enable physics on the platforms group
        platforms.enableBody = true;
        // Here we create the ground.
        var ground = platforms.create(0, game.world.height - 64, 'ground');
        //  Scale it to fit the width of the game (the original sprite is 400x32 in size)
        ground.scale.setTo(2, 2);
        //  This stops it from fallin     
        ground.body.immovable = true;
        //  Now let's create two ledges
        var ledge = platforms.create(400, 400, 'ground');
        ledge.body.immovable = true;
        ledge = platforms.create(-150, 250, 'ground');
        ledge.body.immovable = true;
        scoreText = game.add.text(16, 16, 'score: 0', { fontSize: '32px', fill: '#000' });
     
  
        //add player
        //the first 2 arguments are x and y, then the last is the key of your image
        player = game.add.sprite(32, game.world.height - 160, 'hp');
        //enable physics on player
        game.physics.arcade.enable(player);
        player.body.bounce.y = 0.2;        player.body.gravity.y = 300;
        player.body.collideWorldBounds = true;
        player.animations.add('left', [13, 14, 15], 12, true);
        player.animations.add('right', [26, 27, 28], 12 , true);
        //  Our controls.
        cursors = game.input.keyboard.createCursorKeys();
        gems = game.add.group();
        gems.enableBody = true;
        sword= game.add.group();
        sword.enableBody = true;
        timer= game.time.create(); 
        timer.start();
        timer.loop(750, creategems);
        timer.loop(750,  createsword);
        
        //  Here we'll create 12 of them evenly spaced apart
        
    }
    //Update is continously called while the game is being played - add things like
    //tracking arrow keys, etc here!body.velocity
    function update() {
    //check if player is touching the platform - stops from falling through ground
    game.physics.arcade.collide(player, platforms);
    game.physics.arcade.collide(gems, platforms);
    game.physics.arcade.overlap(player, gems, collectgems, null, this);
    game.physics.arcade.overlap(player, sword, collectsword, null, this);
      //  Reset the players velocity (movement)
      player.body.velocity.x = 0;
        //left key control
        if (cursors.left.isDown)
        {
            //  Move to the left
            player.body.velocity.x = -150;
            6
            player.animations.play('left');
        }
        //right key control
        else if (cursors.right.isDown)
        {
            //  Move to the right
            player.body.velocity.x = 150;
            player.animations.play('right');
        }
        //no keys pressed
        else
        {  //  Add and update the score       
            //  Stand still
            player.animations.stop();
            player.frame = 0;
        }
        //  Allow the player to jump if they are touching the ground.
        if(cursors.up.isDown)
        {
            player.body.velocity.y = -350;
        }
        
            
    }
    //add any extra functions you need here! 
    function collectgems (player, gems) {
    // Removes the star from the screen
    gems.kill();
     
    //  Add and update the score
    score += 10;
    scoreText.text = 'Score: ' + score;
}
    function collectsword (player, sword) {
    // Removes the star from the screen
    sword.kill();
    score -= 30;
    scoreText.text = 'Score: ' + score;
    }
    function creategems() {  //  Add and update the score
       
    var xLocation= Math.random() * 800;
    var gem= gems.create(xLocation, 0, 'gems');
    
    
    //let gravity do its thing
    gem.body.gravity.y = 100;
    //This just gives eac100h snitch a slightly random bounce value
    gem.body.bounce.y = 0.7 + Math.random()
    *0.2;
    
}
 function createsword() {
    var xLocation= Math.random() * 800;
    var shaft= sword.create(xLocation, 0, 'sword');
    
    
    //let gravity do its thing
    shaft.body.gravity.y = 100;
       
    //This just gives eac100h snitch a slightly random bounce value
    shaft.body.bounce.y = 0.7 + Math.random()
    *0.2;}
</script>

</body>
</html> 
