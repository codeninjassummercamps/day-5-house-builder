### @diffs true

# Day 5 House Builder

```template
let startPosition: Position = null
player.onChat("buildHouse", function (w, l, h) {
    agent.teleport(player.position(), WEST)
    agent.move(RIGHT, 1)
    startPosition = agent.getPosition()
    player.say("House Complete :)")
})
loops.forever(function () {
    if (agent.getItemCount(1) <= 1) {
        agent.setItem(PLANKS_OAK, 64, 1)
    }
})
```

## House Builder

Welcome to the last main activity of this week of camp! Let's teach the agent to build a house. We'll start with the easier parts and work our way to the harder parts! This activity has a lot of steps and a lot of code, so make sure you are checking the hints and asking for help when you need it!

## Step 1 

You've been provided 2 pieces of starter code. One is a forever loop. It's job is to make sure that the agent will always have blocks to build with. Feel free to customize the block to whatever building block you prefer! The other code is a chat command that currently moves the agent to a good starting position, notes what that position is in a variable, then tells you that the house is complete. Hmm, doesn't seem like a house is built yet, though.

## Step 2

Our first goal is to make a floor. We already made a checker board floor in a previous activity, so this should be a piece of cake! First, create a new ``||functions.function||`` named "buildFloor" or something similar, and add 2 numbers: width and length. If you aren't sure how to add numbers to a function, make sure to ask a Sensei!

```blocks
function buildFloor (width: number, length: number) { 
}
```

## Step 3

Now that we have a function, let's create our algorithm to build the floor. First, make sure that the agent is in the right position and facing the right direction. We can use the startPosition variable to help! After we've added that, we'll need some sort of way to repeat placing blocks for the width and height. (Hint: this is what we did for the checker builder!)

```blocks
function buildFloor (width: number, length: number) { 
    agent.teleport(startPosition, WEST)
    for (let index = 0; index < width; index++) {
        for (let index = 0; index < length; index++) {
        }      
    }
}
    
```

## Step 4

To build the floor, we'll need to get rid of the grass in the ground under the agent and replace it with the block that the agent is holding. Add these 2 blocks!

```blocks
function buildFloor (width: number, length: number) {
    agent.teleport(startPosition, WEST)
    for (let index = 0; index < width; index++) {
        for (let index = 0; index < length; index++) {
            agent.destroy(DOWN)
            agent.place(DOWN)
        }
    }
}
```

## Step 5

Once we destroy and replace a block, the agent needs to go to the next position, add the block that will move it forward.

```blocks
function buildFloor (width: number, length: number) {
    agent.teleport(startPosition, WEST)
    for (let index = 0; index < width; index++) {
        for (let index = 0; index < length; index++) {
            agent.destroy(DOWN)
            agent.place(DOWN)
            agent.move(FORWARD, 1)
        }
    }
}
```

## Step 6

Awesome! Now the agent will be able to finish a row of the floor! But just like with our Checker Builder, the agent needs to reset back to the start of the row and step to the side before it can build the next row. We'll need 2 move blocks and we will need to use the length as the distance to move back!

```blocks
function buildFloor (width: number, length: number) {
    agent.teleport(startPosition, WEST)
    for (let index = 0; index < width; index++) {
        for (let index = 0; index < length; index++) {
            agent.destroy(DOWN)
            agent.place(DOWN)
            agent.move(FORWARD, 1)
        }
        agent.move(RIGHT, 1)
        agent.move(BACK, length)
    }
}
```

## Step 7

That's all for the floor, but if we tried to test right now the agent wouldn't build our floor! We still need to *call* the function we just made. Add that block right before the ``||player.say||`` block in the chat command.

```blocks
player.onChat("buildHouse", function (w, l, h) {
    agent.teleport(player.position(), WEST)
    agent.move(RIGHT, 1)
    startPosition = agent.getPosition()
    buildFloor(w, l)
    player.say("House Complete :)")
})
function buildFloor (width: number, length: number) {
    agent.teleport(startPosition, WEST)
    for (let index = 0; index < width; index++) {
        for (let index = 0; index < length; index++) {
            agent.destroy(DOWN)
            agent.place(DOWN)
            agent.move(FORWARD, 1)
        }
        agent.move(RIGHT, 1)
        agent.move(BACK, length)
    }
}
```

## Step 8

Our next function to make is going to be the ceiling, since the walls are going to be a bit more tricky. Let's start by making the function "buildCeiling" that has 3 numbers: width, length, and height.

```blocks
function buildCeiling (width: number, length: number, height: number) {
    
}
```

## Step 9

This function is super similar to the floor, but we won't need to destroy any blocks. Let's copy what we had for the floor, but remove the ``||agent.destroy||`` block.

```blocks
function buildCeiling (width: number, length: number, height: number) {
    agent.teleport(startPosition, WEST)
    for (let index = 0; index < width; index++) {
        for (let index = 0; index < length; index++) {
            agent.place(DOWN)
            agent.move(FORWARD, 1)
        }
        agent.move(RIGHT, 1)
        agent.move(BACK, length)
    }
}
```

## Step 10

There's one more difference between a floor and a ceiling: ceilings are up on top of the walls! We need to change the starting position of the agent to make sure it builds at the right height. You'll need the add block from the ``||positions.positions||`` section, then place the ``||variables.startPosition||`` variable in the first part of the plus (replace all 3 of the first blanks). What we need to add to the position is the height plus one, so grab the plus operator from ``||logic.logic||``. Place in the height on the left and type 1 on the right. Then put that operation in the middle of the remaining 3 blanks of the position addition. Use the hint to make sure you've got everything placed correctly!

```blocks
function buildCeiling (width: number, length: number, height: number) {
    agent.teleport(positions.add(
    startPosition,
    pos(0, height + 1, 0)
    ), WEST)
    for (let index = 0; index < width; index++) {
        for (let index = 0; index < length; index++) {
            agent.place(DOWN)
            agent.move(FORWARD, 1)
        }
        agent.move(RIGHT, 1)
        agent.move(BACK, length)
    }
}
```

## Step 11

Now the floor and ceiling functions are done, but our chat command still only calls for the agent to make the floor. That's okay, we'll teach it to build walls first. This is the trickiest section, but just like before, start by making a function. Call it "buildWalls" and give it 3 numbers: width, length,and height.

```blocks
function buildWalls (width: number, length: number, height: number) {
}
```

## Step 12

Alright, now's the part where you really prove what you know, ninja. In the next couple of steps, you will be given a description of what to do, but not specific blocks. It's quite tricky, but remember that the hints are there to help! First, make the agent go to the right starting place (1 block + 1 variable).

```blocks
function buildWalls (width: number, length: number, height: number) {
    agent.teleport(startPosition, WEST)
}
```

## Step 13

Rows of the wall will need to be made as many times as the height number provided, but inside of that, you'll also need to take some actions as many times as the length provided. (2 blocks + 2 variables)

```blocks
function buildWalls (width: number, length: number, height: number) {
    agent.teleport(startPosition, WEST)
    for (let index = 0; index < height; index++) {
        for (let index = 0; index < length; index++) {
        }
    }
}
```

## Step 14

Before beginning to place blocks for the row, the agent should be move up enough to place block under itself. (1 block)

```blocks
function buildWalls (width: number, length: number, height: number) {
    agent.teleport(startPosition, WEST)
    for (let index = 0; index < height; index++) {
        agent.move(UP, 1)
        for (let index = 0; index < length; index++) {
        }
    }
}
```

## Step 15

The agent is ready to start building the first wall, it needs to place blocks and move! (2 blocks)

```blocks
function buildWalls (width: number, length: number, height: number) {
    agent.teleport(startPosition, WEST)
    for (let index = 0; index < height; index++) {
        agent.move(UP, 1)
        for (let index = 0; index < length; index++) {
            agent.place(DOWN)
            agent.move(FORWARD, 1)
        }
    }
}
```

## Step 16

The agent needs to get back in line with the next wall to build. It needs to step back and to the right. (2 blocks)

```blocks
function buildWalls (width: number, length: number, height: number) {
    agent.teleport(startPosition, WEST)
    for (let index = 0; index < height; index++) {
        agent.move(UP, 1)
        for (let index = 0; index < length; index++) {
            agent.place(DOWN)
            agent.move(FORWARD, 1)
        }
        agent.move(BACK, 1)
        agent.move(RIGHT, 1)
    } 
}
```

## Step 17

The agent is getting ready to build the next wall to the right, but this wall needs to be 2 blocks shorter than the width provided, but then it can start placing blocks and moving. (4 blocks + 1 variable)

```blocks
function buildWalls (width: number, length: number, height: number) {
    agent.teleport(startPosition, WEST)
    for (let index = 0; index < height; index++) {
        agent.move(UP, 1)
        for (let index = 0; index < length; index++) {
            agent.place(DOWN)
            agent.move(FORWARD, 1)
        }
        agent.move(BACK, 1)
        agent.move(RIGHT, 1)
        for (let index = 0; index < width - 2; index++) {
            agent.place(DOWN)
            agent.move(RIGHT, 1)
        }
    }
}
```

## Step 18

Next wall, similar code. This time, we're building backwards the whole length provided. (3 blocks + 1 variable)

```blocks
function buildWalls (width: number, length: number, height: number) {
    agent.teleport(startPosition, WEST)
    for (let index = 0; index < height; index++) {
        agent.move(UP, 1)
        for (let index = 0; index < length; index++) {
            agent.place(DOWN)
            agent.move(FORWARD, 1)
        }
        agent.move(BACK, 1)
        agent.move(RIGHT, 1)
        for (let index = 0; index < width - 2; index++) {
            agent.place(DOWN)
            agent.move(RIGHT, 1)
        }
        for (let index = 0; index < length; index++) {
            agent.place(DOWN)
            agent.move(BACK, 1)
        }
    }
}
```

## Step 19

The agent needs to reposition itself again to prepare for the next wall, have it step forward and left! (2 blocks)

```blocks
function buildWalls (width: number, length: number, height: number) {
    agent.teleport(startPosition, WEST)
    for (let index = 0; index < height; index++) {
        agent.move(UP, 1)
        for (let index = 0; index < length; index++) {
            agent.place(DOWN)
            agent.move(FORWARD, 1)
        }
        agent.move(BACK, 1)
        agent.move(RIGHT, 1)
        for (let index = 0; index < width - 2; index++) {
            agent.place(DOWN)
            agent.move(RIGHT, 1)
        }
        for (let index = 0; index < length; index++) {
            agent.place(DOWN)
            agent.move(BACK, 1)
        }
        agent.move(FORWARD, 1)
        agent.move(LEFT, 1)
    }
}
```

## Step 20

In the last stretch! One more section of code for the last wall. This time, build left for the width minus 2. (4 blocks + 1 variable)

```blocks
function buildWalls (width: number, length: number, height: number) {
    agent.teleport(startPosition, WEST)
    for (let index = 0; index < height; index++) {
        agent.move(UP, 1)
        for (let index = 0; index < length; index++) {
            agent.place(DOWN)
            agent.move(FORWARD, 1)
        }
        agent.move(BACK, 1)
        agent.move(RIGHT, 1)
        for (let index = 0; index < width - 2; index++) {
            agent.place(DOWN)
            agent.move(RIGHT, 1)
        }
        for (let index = 0; index < length; index++) {
            agent.place(DOWN)
            agent.move(BACK, 1)
        }
        agent.move(FORWARD, 1)
        agent.move(LEFT, 1)
        for (let index = 0; index < width - 2; index++) {
            agent.place(DOWN)
            agent.move(LEFT, 1)
        }
    }
}
```

## Step 21

You survived! That was all we needed for building the walls! Time to add the function calls to our chat command. Call ``||functions.buildWalls||``, then ``||functions.buildCeiling||`` at the bottom of the chat command before the ``||player.say||`` block. Don't forget to put in the variables in the right slots! w, l, and h from the chat command need to be placed in the function calls in the right order for the width, length, and height!

```blocks
player.onChat("buildHouse", function (w, l, h) {
    agent.teleport(player.position(), WEST)
    agent.move(RIGHT, 1)
    startPosition = agent.getPosition()
    buildFloor(w, l)
    buildWalls(w, l, h)
    buildCeiling(w, l, h)
    player.say("House Complete :)")
})
let startPosition: Position = null
// Width is X direction
// Length is Z direction
function buildFloor (width: number, length: number) {
}
function buildWalls (width: number, length: number, height: number) {
}
function buildCeiling (width: number, length: number, height: number) {
}
```

## Last Step

You can use the hint on this step to check your final code, but the hint is really big and hard to read. You can also try looking back at the hints in previous steps if you need. Otherwise, go and test your buildHouse command! Try starting with a very small house, like 3 by 5 by 4 so that the building doesn't take too long!

```blocks
    let startPosition: Position = null
// Width is X direction
// Length is Z direction
function buildFloor (width: number, length: number) {
    agent.teleport(startPosition, WEST)
    for (let index = 0; index < width; index++) {
        for (let index = 0; index < length; index++) {
            agent.destroy(DOWN)
            agent.place(DOWN)
            agent.move(FORWARD, 1)
        }
        agent.move(RIGHT, 1)
        agent.move(BACK, length)
    }
}
function buildWalls (width: number, length: number, height: number) {
    agent.teleport(startPosition, WEST)
    for (let index = 0; index < height; index++) {
        agent.move(UP, 1)
        for (let index = 0; index < length; index++) {
            agent.place(DOWN)
            agent.move(FORWARD, 1)
        }
        agent.move(BACK, 1)
        agent.move(RIGHT, 1)
        for (let index = 0; index < width - 2; index++) {
            agent.place(DOWN)
            agent.move(RIGHT, 1)
        }
        for (let index = 0; index < length; index++) {
            agent.place(DOWN)
            agent.move(BACK, 1)
        }
        agent.move(FORWARD, 1)
        agent.move(LEFT, 1)
        for (let index = 0; index < width - 2; index++) {
            agent.place(DOWN)
            agent.move(LEFT, 1)
        }
    }
}
function buildCeiling (width: number, length: number, height: number) {
    agent.teleport(positions.add(
    startPosition,
    pos(0, height + 1, 0)
    ), WEST)
    for (let index = 0; index < width; index++) {
        for (let index = 0; index < length; index++) {
            agent.place(DOWN)
            agent.move(FORWARD, 1)
        }
        agent.move(RIGHT, 1)
        agent.move(BACK, length)
    }
}
loops.forever(function () {
    if (agent.getItemCount(1) <= 1) {
        agent.setItem(PLANKS_OAK, 64, 1)
    }
})
player.onChat("buildHouse", function (w, l, h) {
    agent.teleport(player.position(), WEST)
    agent.move(RIGHT, 1)
    startPosition = agent.getPosition()
    buildFloor(w, l)
    buildWalls(w, l, h)
    buildCeiling(w, l, h)
    player.say("House Complete :)")
})

```

## Activity Complete!

Awesome job, ninja! You've got a house-building agent, now, and you've proved you're a coding expert!
