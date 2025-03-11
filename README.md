# Hi there 👋 I'm Matt.

I'm a second year student in NSCC's game programming program specializing in gameplay and generalist programming. I've dabbled in software engineering, IT, and management best practices, but my true love is game programming. I know a few different programming languages and am always hungry for more experience in the industry!

## 💼 I’m currently working on...
Right now I'm working on a game called "Cauldron Chaos". It's a little game where you run a potion shop similarly to the game "Overcooked". The aim of the game was to create a version of the game that had less time pressure and focused more on having chaos injected into the gameplay to make goofy scenarios. You play as the alchemist's apprentice and your goal is to run the shop for two weeks while the alchemist is away. Funny scenarios like the cauldrons gaining sencience or the alchemist's inlaws (who are slugs) coming over help inject chaos and challenges to the levels. The game isn't on any distribution websites yet, but you can check out the code for it on [Github](https://www.youtube.com/watch?v=ZmUdKNLKnow), or check the [release section](https://github.com/adorey91/CauldronChaos/releases) for a build of the project if you want to check it out.

![Screenshot 2025-03-11 201045](https://github.com/user-attachments/assets/cb6f6ac6-c872-4217-8c79-5ae941b010b7)

I am the Audio Designer for the game, so I've been in charge of everything relating to audio, from the designing of the audio manager to the actual sounds and music used in the game. I've also contributed to the programing of some systems in the game. These systems include the input manager, player movement system, player interaction system, and some systems which have sadly been relegated to the cutting room floor. For this game I really focused on trying out a new architecture for my managers. I used a static instance to make the manager accissble to all scripts as well as implementing some code to make the object spawn itself if no istance already exists. Here's a sample below showing that off.

```
    private static AudioManager _instance;

    //function that checks if Instance exists and spawns one if it does not
    public static AudioManager instance
    {
        get
        {
            _instance = FindObjectOfType<AudioManager>();  // Try to find an existing AudioManager in the scene

            //check if Instance is null
            if (_instance == null)
            {
                // If no Instance exists, instantiate it
                _instance = Instantiate(Resources.Load("AudioManager") as GameObject).GetComponent<AudioManager>();
                _instance.name = "AudioManager";
            }
        return _instance;
        }
    }

    // Awake is called before the first frame update and before start
    void Awake()
    {
        //check if this is the active Instance
        if (_instance == null)
        {
            _instance = this;
            DontDestroyOnLoad(this);
        }
        else
        {
            //remove copy
            Destroy(gameObject);
        }
    }
```

## 📂 Things I've done...
I've done a few things in the field of programming. here are some highlights.

### 🎮 Game & Program Projects:
Here are some of the games I've made.

#### Burrow Blaster
My main project for the last semester a SHMUP (Shoot em' Up) I made for my Game Development III course called Burrow Blaster. The idea with the game is to make a more accessible SHMUP by combining it with upgrade style mechanics in the vein of 2000s internet flash games (such as "*Learn to Fly*"). This is to help people get through the wall of "Get Good" that these games often require. I've included a video showing off the game below (access by clicking the picture).

[![image](https://github.com/user-attachments/assets/9ab1f757-0d1c-432c-af1b-f31ae8294306)](https://www.youtube.com/watch?v=aEwcz2bqGlM)

The game was developed in collaboration with one of the game art students in the program. I did all the programming as well as taking on tasks around sound design, UI design, as well as gameplay and level design. There's a lot I'm pretty proud of in this project, but one of the things I'm most proud of, is the code handling visuals for player upgrades. Here's a snippet of that code that handles updating player visuals.

I'm also very happy with my gameplay scene management system. It helped me fix issues with game performance from having too much cramed in the gameplay scene, and functions through loading and unloading chunks of the gameplay scene as they are needed. Here's a snippet showing off the logic it goes through.

```
void Update()
{
    float distancefromPlayer;

    //loop through tiles to load them in
    for (int i=0; i<segments.Length; i++)
    {
        //check distance from player camera
        distancefromPlayer = Mathf.Abs(segments[i].position.z - cam.transform.position.z);
        //Debug.Log(distancefromPlayer);

        //load if close enough and unloaded
        if (distancefromPlayer <= distance && !segments[i].loaded)
        {
            //Debug.Log("Calling load segment");
            LoadSegment(segments[i]);
            segments[i].loaded = true;
        }
        //unloadd scene if too far
        else if (distancefromPlayer > distance && segments[i].loaded)
        {
            UnloadSegment(segments[i]);
            segments[i].loaded = false;
        }
    }
}

//Function to load segments if player is close to segment position;
private void LoadSegment(Segment segment)
{
    AsyncOperation asyncOperation = SceneManager.LoadSceneAsync(segment.scene, LoadSceneMode.Additive);
}

//Function to un-load segments if player is far from segment position;
private void UnloadSegment(Segment segment)
{
    AsyncOperation unloadOperation = SceneManager.UnloadSceneAsync(segment.scene);
}
```
Though the game is still a work in progress it's avaiable to play on [itch.io](https://gdmbenedict.itch.io/burrow-blaster), and if you have time I'd be delighted to get some feedback!

#### Proceedural City Generation Program
This is a project I worked on for my proceedural generation class. The goal of the project was to make a proceeduraly generate city scape. I did this by combining three major steps in its generation.
1. Decide city block dimensions by randomly placing roads along the edges of the city.
2. Populate city block by placing buildings of random sizes in all available and allowed locations (ex: must be adjacent to a road).
3. Generate height for the building according to height rules (ex: larger buildings more likely to grow).

I was particularly proud of the building generation and the proceedural determination of what building block to use. Below is a snippet of code showing that off.

```
    //function that spawns a building visual from a map position
    private void SpawnBuilding(int z, int x, int lengthZ, int lengthX, int height)
    {
        Vector3 position;
        GameObject instance;


        int orientation;
        Quaternion rotation;

        for (int i=z; i<z+lengthZ; i++)
        {
            for (int j=x; j<x+lengthX; j++)
            {
                //setting position
                position = new Vector3(j - widthX / 2, height - 1, i - widthZ / 2);

                //resetting orientation
                orientation = 0;

                //determining orientation using truth table
                if (j + 1 < x + lengthX) {
                    orientation += 1;
                }
                if (j - 1 >= x)
                {
                    orientation += 2;
                }
                if (i + 1 < z + lengthZ)
                {
                    orientation += 4;
                }
                if (i -1 >= z)
                {
                    orientation += 8;
                }

                //using orientation to spawn visual in right position
                switch (orientation)
                {
                    //All possible cases for orientation
                }
            }
        }
    }
```

Below is a video showcasing the final results of the project in action, which you can click on the image to watch. You can also check out the code yourself by checking out the [github](https://github.com/gdmbenedict/Proprietary-Algorithm/tree/main/Assets/Scripts) for the project.

[![image](https://github.com/user-attachments/assets/7485d65a-85e4-4afe-b511-a4b9f021f871)](https://www.youtube.com/watch?v=bKUf-9amz4g)

#### Gish
This project is one that I made as part of my Game Programming classes at NSCC. It is a text RPG game in the style of older computer rpgs like Rogue, Net-hack, and Moria. I made it completely from scratch, programming in simple enemy AI, loading from text documents, items, and other mechanics. The main focus of this project was focusing on OOP and the architecture of code in games. I really liked this project because it's made me appreciate how much needs to go into even simple mechanics I've taken for granted. 

![PUJLSr](https://github.com/user-attachments/assets/4b50cbee-9f02-4d05-bfbc-232ae5851aca)

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
If you're interested in what you're seeing you can check out the game on [itch.io](https://gdmbenedict.itch.io/gish) or check out the code on my [github](https://github.com/gdmbenedict/TextRPG_V2).

### 🕹️ Game Jams:
Here are some Game Jams I've participated in.

#### Global GameJam 2025: Bubblin'Up
I took part in thje Global GameJam for the year 2025. The propmt for the GameJam was "Bubble". My team decided to take a more literal approach to this and we made a 2D bubble based puzzle platformer where you manipulate your bubble size. The idea was to manipulate your size to achieve different objectives. like fitting through a small gap, or inflating your size to jump higher. I handled the creating the architecture and support systems for the game. In specific, for prgramming tasks, I made the audio manager, level manager, game manager, and UI manager for the game. I also created the UI for the game and edited all the SFX and Music for the game. For this GameJam my big challenge was working with people who were unfamiliar with development tools such as Github and Unity. Teaching others develop games was a once in a lifetime experience, but because of the time pressure I hope it stays once in my lifetime. The game is up on the [Global GameJam Website](https://globalgamejam.org/games/2025/bubblin-3) if you want to check it out. You can also take a look at the code on [Github](https://github.com/gdmbenedict/Bubbling-Up) if that's more your speed.

![Bubblin' Up Grow](https://github.com/user-attachments/assets/dcf4f0e8-98ab-4872-81b0-2195fe5f73ca)

#### Summer MelonJam 2024: When Life Gives You Lemmings:
I took part Summer MelonJam 2024 with a group of students over our summer break. The goal was to create a game fitting the prompt "Flow". While there were some more direct ways to interpret the prompt, we decided to try to be a little more creative. So we made a game about directing the flow of lemmings through the flow of traffic. This was inspired by the team's shared experience with reaching "Flow State" primarily in rythm games, so we made a mash-up of a game like "_Crossy Road_" and a rythm game. I did all the programing for the game, and though it was tough, I felt the challenge of reliably timing things to a beat was really interesting to solve. The game is up on [itch.io](https://gdmbenedict.itch.io/when-life-gives-you-lemmings) and is available to be player in browser if you want to check it out.

![image](https://github.com/user-attachments/assets/8c767d92-a08c-4c46-bc7d-6c43f41b6ce4)

#### GMTK GameJam-2023: WorldWide Casino:
In the GMTK GameJam-2023 I helped make a game called "World Wide Casino" (found [here](https://arizoba.itch.io/worldwide-casino) on itch.io). For the prompt "Rolls Reversed" we made a game where you play as the boss in an old "Time Crisis" style shooting game. You must avoid the player's shots and make your way back to your casino to retrieve your gun and turn the tables against the player.

![WorldWideCasino](https://github.com/gdmbenedict/gdmbenedict/assets/97464794/70073a7b-f6cb-4157-9b94-06cbbece7293)

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
  <img src="https://github.com/devicons/devicon/blob/master/icons/godot/godot-original.svg" title="Godot" alt="Godot" width="100" height="100"/>&nbsp;
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
