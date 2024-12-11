# Hi there 👋 I'm Matt.

I'm Matthieu Benedict, a second year student in NSCC's game programming progam specializing in gameplay and generalist programming. I've dabbled in software engineering, IT, and management best practices, but my true love is game programming. I know a few different programming languages and am always hungry for more experience in the industry!

## 💼 I’m currently working on...
My main project at the moment is making a game design document for a theoretical horror game, done as part of my Game Design class at NSCC. The concept of the game is to have a survival horror game in the vein of "Silent Hill" that focueses on ARG elements in its gameplay. If you're interested in the concept, then you can check out the documentation I've made on [this](https://github.com/gdmbenedict/GDD-Horror-Game/wiki) Github wiki. I've also made a video walking through the blockmesh (graybox/whitebox) of the first three levels. You can watch that by clicking on the video below.

[![BlockMesh Video](https://img.youtube.com/vi/IHgyOoURJ6I/0.jpg)](https://www.youtube.com/watch?v=IHgyOoURJ6I)

## 📂 Things I've done...
I've done a few things in the field of programming. here are some highlights.

### 🎮 Game Projects:
Here are some of the games I've made.

#### Shooter Tech Demo
This is a project I have worked on as part of my Game Mechanics class at NSCC. This was a solo development project meant to test my skills in applying programming to create game systems using an engine. Some of the mechanics implemented in this tech demo were: Enemy AI states (Patrolling, Searching, Chasing, Attacking, and Retreating), instantiating projectiles, collectables, a respawn system, exploding projectiles, and wall running. I also added increased physics capabilities to the tech demo as it relates to the player by making a custom controller using Unity's physics Rigidbody system. I've included a code snippet handling wall jumping from the wall running script below.

```
    public void WallJump(InputAction.CallbackContext context)
    {
        if (isWallRunning && !gameManager.isGamePaused() && context.performed)
        {
            if (wallLeft)
            {
                Vector3 wallRunJumpDirection = transform.up + leftWallHit.normal;

                //reset Y velocity to fix small jump bug
                rb.velocity = new Vector3(rb.velocity.x, 0f, rb.velocity.z);

                //calculate jump velocity and apply it to normalized wall jump vector
                float jumpVelocity = Mathf.Sqrt(wallRunJumpHeight * -2f * Physics.gravity.y);
                rb.AddForce(wallRunJumpDirection.normalized * jumpVelocity, ForceMode.VelocityChange);
            }
            else if (wallRight)
            {
                Vector3 wallRunJumpDirection = transform.up + rightWallHit.normal;

                //reset Y velocity to fix small jump bug
                rb.velocity = new Vector3(rb.velocity.x, 0f, rb.velocity.z);

                //calculate jump velocity and apply it to normalized wall jump vector
                float jumpVelocity = Mathf.Sqrt(wallRunJumpHeight * -2f * Physics.gravity.y);
                rb.AddForce(wallRunJumpDirection.normalized * jumpVelocity, ForceMode.VelocityChange);
            }
        }
    }
```

I also made a video showcasing what has been implemented in the tech demo. If you want to see it in motion you can watch it by clicking on the video below.

[![TechDemo Video](https://img.youtube.com/vi/lDq96HGjxyw/0.jpg)](https://www.youtube.com/watch?v=lDq96HGjxyw)

I am still working on adding more polish to this project, and it will go up on Itch.io in the near future if you're interested in checking it out.

#### Text RPG
This project is one that I made as part of my Game Programming class at NSCC. It is a text RPG game in the style of older computer rpgs like Rogue, Net-hack, and Moria. I made it completely from scratch, programming in simple enemy AI, loading from text documents, items, and other mechanics. The main focus of this project was focusing on OOP and the architecture of code in games. I really liked this project because it's made me appreciate how much needs to go into even simple mechanics I've taken for granted. 

![TextRPG](https://github.com/gdmbenedict/gdmbenedict/assets/97464794/9ca39f96-5ddd-4b3f-b589-0e5198a53afb)

One thing I'm proud on having implemented is an action system that allows entities in the game to take multiple actions within their turn according to their speed. This means that other entities can both be faster or slower than the player, in a manner similar to how a board game like Dungeons and Dragons does their turn system. I've included the code for turns below as a peek in to the system.

```
/// <summary>
/// Method that instructs the EntityTurn to go through its update process.
/// </summary>
/// <param name="map">the map the game is on</param>
/// <param name="uIManager">the manager for the game UI</param>
/// <param name="itemManager">the manager for the items on the map</param>
/// <param name="entityManager">the manager for entities on the map</param>
/// <returns></returns>
public bool Update(Map map, UIManager uIManager, ItemManager itemManager, EntityManager entityManager)
{
    //adding speed to build up the Entity's turn
    turnBuildup += entity.spd.GetStat();

    while (turnBuildup >= GlobalVariables.actionThreshold)
    {
        //update UI for player
        if (entity == entityManager.GetPlayer())
        {
            uIManager.DrawUI(map);
        }

        //checks if player is standing on the exit tile
        if (map.GetTile(map.GetEntityIndex(entityManager.GetPlayer())).GetExit())
        {
            return true;
        }

        //adds events of the turn to the log (if the events were notable)
        uIManager.AddEventToLog(TakeAction(map, uIManager, itemManager));

        //check if entity takes damage from tile
        if (map.GetTile(map.GetEntityIndex(entity)).GetDangerous())
        {
            uIManager.AddEventToLog(map.GetTile(map.GetEntityIndex(entity)).DealDamage(entity));
        }

        //gets the entity manager to check for dead enemies
        entityManager.CheckDeadEntities(map, uIManager);
    }

    return false;

}
```

This is still a project in active development so it's not available as a playable build, but you can take a look at the code on its publically available github repository [here](https://github.com/gdmbenedict/TextRPG_V2). *Comments in the code are pending.

#### Haunted Hospital
This is another one of my NSCC solo projects. This assignment was for NSCC's "Game Engine" course, and was an assignment designed to teach Unity's sequencing and trigger systems, though I decided to expand on what the assignment was doing by trying to implement aspects of environmental story telling to the level. My main goal with this project was to evoke a foreboding/creepy atmosphere using Unity's lighting systems, sound systems, triggers, and timelines. Another key aspect I tried to keep in mind was the significance of using closed doors, and flickering lights as a visual motif in the level design. The game can be found [here](https://twitchton.itch.io/hospital-horror) on itch.io if you would like to see how the atmosphere of the game turned out for yourself.

![Screenshot 2023-12-12 224759](https://github.com/gdmbenedict/gdmbenedict/assets/97464794/e00b40a1-ca96-424e-bb88-905245957e84)

If you just want to see smoeone play it, you can thanks to the kind people who have played the game and put up videos of it on youtube. You can watch the videos by clicking on the video below.

[![Hospital Horror Videos](https://img.youtube.com/vi/aiAVZlFRoz8/0.jpg)](https://www.youtube.com/watch?v=aiAVZlFRoz8&list=PLj5bBtNjiCmnI7jya1zTzuOpzrbn9Z07t)

### 🕹️ Game Jams:
Here are some Game Jams I've participated in.

#### Global GameJam-2024: Dad-Sim 1997:
In the Global GameJam-2024 I took part in a team effort of some NSCC students and a Dalhousie student to create a game with the prompt "Make me Laugh". Our group decided to make a goofy physics based collection of minigames, in the style of a game like "Octodad". The I contributed programming for the player controls, some of the mini-games, as well as the UI and menus for the game. Unfortunetly, we were not able to complete our vision for the game by the end of the Jam, but I've gotten together with some members of the teams to fix some issues with the game and release it on itch.io. In the meantime, what we were able to complete by the GameJam deadline can be found on the official Global gameJam website [here](https://globalgamejam.org/games/2024/dad-simulator-1997-4).

![DadSim](https://github.com/gdmbenedict/gdmbenedict/assets/97464794/fa5ef64c-2135-4e25-b4f0-c4ba37db35d9)

#### GMTK GameJam-2023: WorldWide Casino:
In the GMTK GameJam-2023 I helped make a game called "World Wide Casino" (found [here](https://arizoba.itch.io/worldwide-casino) on itch.io). For the prompt "Rolls Reversed" we made a game where you play as the boss in an old "Time Crisis" style shooting game. You must avoid the player's shots and make your way back to your casino to retrieve your gun and turn the tables against the player.

![WorldWideCasino](https://github.com/gdmbenedict/gdmbenedict/assets/97464794/70073a7b-f6cb-4157-9b94-06cbbece7293)

#### DIG GameJam 2023: Mutual Card Game:
The DIG GameJam is a game jam run locally (in Halifax) through the Dalhousie Interactive Games Society, a Dalhousie club for students interested in persuing game creation. This was the first GameJam hosted by them, and the theme for the GameJam was "Mutual Gain". I teamed up with some of my peers at NSCC to make a game for this GameJam, and together we came up with the idea of a card game based off a trading mechanic. The idea was players would have to work with their hand of cards to get the highest point total, collected by getting numbered cards of the player's suit, by trading with other players and using power cards (face cards). If You're interested you can find the game on [itch.io](https://twitchton.itch.io/mutual-card-gain).

![MutualCardGain](https://github.com/gdmbenedict/gdmbenedict/assets/97464794/8bfc5663-b4bf-4ca6-a6f5-f17c76fcfe46)

### 📋 Other:
I've also gained experience in programming non-game related applications. Currently, I help manage a couple websites for a small company called "Qualiti7". I help manage their [main website](https://qualiti7.com/) as well as their [conferene website](https://iq7conference.com/?lang=en). Feel free to check them out.

## 🏫 Education...
- **NSCC** - Game Programming Diplmoa (2023 - Current) *still in education*
- **UNB** - Bachelor's in Science: Software Engineering (2020 - 2023) *85 Credits completed*.  

## 💪 Some of my skills...

<div>
  <img src="https://github.com/devicons/devicon/blob/master/icons/c/c-original.svg" title="C" alt="C" width="100" height="100"/>&nbsp;
  <img src="https://github.com/devicons/devicon/blob/master/icons/csharp/csharp-original.svg" title="CSharp" alt="C#" width="100" height="100"/>&nbsp;
  <img src="https://github.com/devicons/devicon/blob/master/icons/java/java-original-wordmark.svg" title="Java" alt="Java" width="100" height="100"/>&nbsp;
  <img src="https://github.com/devicons/devicon/blob/master/icons/javascript/javascript-original.svg" title="JavaScript" alt="JavaScript" width="100" height="100"/>&nbsp;
  <img src="https://github.com/devicons/devicon/blob/master/icons/nodejs/nodejs-original-wordmark.svg" title="NodeJS" alt="NodeJS" width="100" height="100"/>&nbsp;
  <img src="https://github.com/devicons/devicon/blob/master/icons/github/github-original-wordmark.svg" title="Github" alt="Github" width="100" height="100"/>&nbsp;
  <img src="https://github.com/devicons/devicon/blob/master/icons/unity/unity-original.svg" title="Unity" alt="Unity" width="100" height="100"/>&nbsp;
</div>

## ✌️ About me...

### Fun facts:
- I am bilingual in French and English. 🥖
- I liked eating so much as a kid that I figured I should learn to cook and bake well. 🍳
- I build plastic figurines of humanoid robots. 🤖
- I like to paint to help me relax. 🖌️
- I like playing pretend with others around a table for long periods of time (D&D). 🐉
- When I have time for it I try to practice the guitar. 🎸

### Media that has influenced me:
#### Games 🖥️
1. Paper Mario TTYD
2. Pyre
3. Nier: Replicant
#### TV 📺
1. Bojack Horseman
2. Breaking Bad
3. Gundam: IBO
#### Movies 🎥
1. Blade Runner 2049
2. UP
3. Everything Everywhere All at Once
#### Books 📖
1. Do Androids Dream of Electric Sheep
2. Dune
3. Chainsaw Man

## 👯 I’m looking to collaborate on ...
Pretty much anything to do with gamedev. If you want my help or to work together send me a message. 👍

## 📫 How to reach me...
[gdmbenedict@outlook.com](mailto:gdmbenedict@outlook.com) <br>
[Linkedin](https://www.linkedin.com/in/matthieu-benedict-646a761a9/) <br>
[Facebook](https://www.facebook.com/gdmbenedict) <br>
[Instagram](https://www.instagram.com/gdmbenedict/) <br>


<!--
**gdmbenedict/gdmbenedict** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
