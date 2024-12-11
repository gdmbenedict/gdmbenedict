# Hi there 👋 I'm Matt.

I'm Matthieu Benedict, a second year student in NSCC's game programming progam specializing in gameplay and generalist programming. I've dabbled in software engineering, IT, and management best practices, but my true love is game programming. I know a few different programming languages and am always hungry for more experience in the industry!

## 💼 I’m currently working on...
My main project at the moment is a SHMUP (Shoot em' Up) I made for my Game Development III course called Burrow Blaster. The idea with the game is to make a more accessible SHMUP by combining it with upgrade style mechanics in the vein of 2000s internet flash games (such as "*Learn to Fly*"). This is to help people get through the wall of "Get Good" that these games often require. I've included a video showing off the game below.

[![image](https://github.com/user-attachments/assets/9ab1f757-0d1c-432c-af1b-f31ae8294306)](https://www.youtube.com/watch?v=aEwcz2bqGlM)

The game was developed in collaboration with one of the game art students in the program. I did all the programming as well as taking on tasks around sound design, UI design, as well as gameplay and level design. There's a lot I'm pretty proud of in this project, but one of the things I'm most proud of, is the code handling visuals for player upgrades. Here's a snippet of that code that handles updating player visuals.

```
//Function that updates the visuals for a badger
private void UpdateVisualsFromList(int upgradeLevel, bool addative, List<GameObject[]> visualsList, bool transition)
{
    //Turn off visuals if visuals are on
    foreach (GameObject[] visuals in visualsList)
    {
        foreach (GameObject visual in visuals)
        {
            if (visual.activeSelf)
            {
                visual.SetActive(false);
            }
        }
    }

    //Determine which types of visual it is (addative or replacing)
    if (addative)
    {
        for (int i=0; i<upgradeLevel+1; i++)
        {
            foreach (GameObject visual in visualsList[i])
            {
                visual.SetActive(true);
            }
        }
    }
    else
    {
        //determine which version of the collection of visuals to use
        switch (upgradeLevel)
        {
            //level 1
            case 1:
                foreach (GameObject visual in visualsList[1])
                {
                    visual.SetActive(true);
                }
                break;

            //level 2
            case 2:
                foreach (GameObject visual in visualsList[2])
                {
                    visual.SetActive(true);
                }
                break;

            //level 3
            case 3:
                foreach (GameObject visual in visualsList[3])
                {
                    visual.SetActive(true);
                }
                break;

            //level 4
            case 4:
                foreach (GameObject visual in visualsList[4])
                {
                    visual.SetActive(true);
                }
                break;

            //level 0 or wrong level
            default:
                foreach (GameObject visual in visualsList[0])
                {
                    visual.SetActive(true);
                }
                break;
        }
    }
```

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

## 📂 Things I've done...
I've done a few things in the field of programming. here are some highlights.

### 🎮 Game & Program Projects:
Here are some of the games I've made.

#### Proceedural City Generation Program
This is a project I worked on for my proceedural generation class. The goal of the project was to make a proceeduraly generate city scape. I did this by combining three major steps in its generation.
1. Decide city block dimensions by randomly placing roads along the edges of the city.
2. Populate city block by placing buildings of random sizes in all available and allowed locations (ex: must be adjacent to a road).
3. Generate height for the building according to height rules (ex: larger buildings more likely to grow).

I was particularly proud of the building generation and the proceedural determination of what building block to use. Below is a snippet of code showing that off.

```
 //function that spawns a road visual from map position
    private void SpawnRoad(float z, float x)
    {
        Vector3 position;
        GameObject instance;
        position = new Vector3(x - widthX / 2, 0, z - widthZ / 2);
        instance = Instantiate(road, position, Quaternion.identity);
        instance.transform.parent = visualsHolder.transform;
    }

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
                    //1x1
                    case 0:
                        rotation = Quaternion.Euler(0, 0, 0);
                        position = new Vector3(x - widthX / 2, height - 1, z - widthZ / 2);
                        instance = Instantiate(building1x1, position, Quaternion.identity);
                        instance.transform.parent = visualsHolder.transform;
                        break;
                        
                    //left end
                    case 1:
                        rotation = Quaternion.Euler(0, 90, 0);
                        instance = Instantiate(buildingEnd, position, rotation);
                        instance.transform.parent = visualsHolder.transform;
                        break;

                    //right end
                    case 2:
                        rotation = Quaternion.Euler(0, 270, 0);
                        instance = Instantiate(buildingEnd, position, rotation);
                        instance.transform.parent = visualsHolder.transform;
                        break;

                    //right-left connector
                    case 3:
                        rotation = Quaternion.Euler(0, 90, 0);
                        instance = Instantiate(buildingConnector, position, rotation);
                        instance.transform.parent = visualsHolder.transform;
                        break;

                    //bottom end
                    case 4:
                        rotation = Quaternion.Euler(0, 0, 0);
                        instance = Instantiate(buildingEnd, position, rotation);
                        instance.transform.parent = visualsHolder.transform;
                        break;

                    //bottom left corner
                    case 5:
                        rotation = Quaternion.Euler(0, 0, 0);
                        instance = Instantiate(buildingCorner, position, rotation);
                        instance.transform.parent = visualsHolder.transform;
                        break;

                    //bottom right corner
                    case 6:
                        rotation = Quaternion.Euler(0, 270, 0);
                        instance = Instantiate(buildingCorner, position, rotation);
                        instance.transform.parent = visualsHolder.transform;
                        break;

                    //bottom wall
                    case 7:
                        rotation = Quaternion.Euler(0, 0, 0);
                        instance = Instantiate(buildingWall, position, rotation);
                        instance.transform.parent = visualsHolder.transform;
                        break;

                    //top end
                    case 8:
                        rotation = Quaternion.Euler(0, 180, 0);
                        instance = Instantiate(buildingEnd, position, rotation);
                        instance.transform.parent = visualsHolder.transform;
                        break;

                    //top left corner
                    case 9:
                        rotation = Quaternion.Euler(0, 90, 0);
                        instance = Instantiate(buildingCorner, position, rotation);
                        instance.transform.parent = visualsHolder.transform;
                        break;

                    //top right corner
                    case 10:
                        rotation = Quaternion.Euler(0, 180, 0);
                        instance = Instantiate(buildingCorner, position, rotation);
                        instance.transform.parent = visualsHolder.transform;
                        break;

                    //top wall
                    case 11:
                        rotation = Quaternion.Euler(0, 180, 0);
                        instance = Instantiate(buildingWall, position, rotation);
                        instance.transform.parent = visualsHolder.transform;
                        break;

                    //up-down connector
                    case 12:
                        rotation = Quaternion.Euler(0, 0, 0);
                        instance = Instantiate(buildingConnector, position, rotation);
                        instance.transform.parent = visualsHolder.transform;
                        break;

                    //left wall
                    case 13:
                        rotation = Quaternion.Euler(0, 90, 0);
                        instance = Instantiate(buildingWall, position, rotation);
                        instance.transform.parent = visualsHolder.transform;
                        break;

                    //right wall
                    case 14:
                        rotation = Quaternion.Euler(0, 270, 0);
                        instance = Instantiate(buildingWall, position, rotation);
                        instance.transform.parent = visualsHolder.transform;
                        break;

                    //middle section
                    case 15:
                        rotation = Quaternion.Euler(0, 0, 0);
                        instance = Instantiate(buildingCenter, position, rotation);
                        instance.transform.parent = visualsHolder.transform;
                        break;

                    //send debug message that an incorrect value was reached
                    default:
                        //Debug.Log("Incorrect calculation of orientation: " + orientation);
                        break;
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
I also added functionality to handle system settings through a json file.

```
//Loading Settings
string path = Path.Combine(Environment.CurrentDirectory, GlobalVariables.settingsDirectory, GlobalVariables.settingsFilename);

if (!File.Exists(path))
{
    Console.WriteLine("Setting file not found");
    Console.ReadKey();
    return;
}

string settingsText = File.ReadAllText(path);
Settings settings = JsonSerializer.Deserialize<Settings>(settingsText);
settings.ApplySettings();
```

If you're interested in what you're seeing you can check out the game on [itch.io](https://gdmbenedict.itch.io/gish) or check out the code on my [github](https://github.com/gdmbenedict/TextRPG_V2).

#### Haunted Hospital
This is another one of my NSCC solo projects. This assignment was for NSCC's "Game Engine" course, and was an assignment designed to teach Unity's sequencing and trigger systems, though I decided to expand on what the assignment was doing by trying to implement aspects of environmental story telling to the level. My main goal with this project was to evoke a foreboding/creepy atmosphere using Unity's lighting systems, sound systems, triggers, and timelines. Another key aspect I tried to keep in mind was the significance of using closed doors, and flickering lights as a visual motif in the level design. The game can be found [here](https://twitchton.itch.io/hospital-horror) on itch.io if you would like to see how the atmosphere of the game turned out for yourself.

![Screenshot 2023-12-12 224759](https://github.com/gdmbenedict/gdmbenedict/assets/97464794/e00b40a1-ca96-424e-bb88-905245957e84)

If you just want to see smoeone play it, you can thanks to the kind people who have played the game and put up videos of it on youtube. You can watch the videos by clicking on the video below.

[![Hospital Horror Videos](https://img.youtube.com/vi/aiAVZlFRoz8/0.jpg)](https://www.youtube.com/watch?v=aiAVZlFRoz8&list=PLj5bBtNjiCmnI7jya1zTzuOpzrbn9Z07t)

### 🕹️ Game Jams:
Here are some Game Jams I've participated in.

#### Summer MelonJam 2024: When Life Gives You Lemmings:
I took part Summer MelonJam 2024 with a group of students over our summer break. The goal was to create a game fitting the prompt "Flow". While there were some more direct ways to interpret the prompt, we decided to try to be a little more creative. So we made a game about directing the flow of lemmings through the flow of traffic. This was inspired by the team's shared experience with reaching "Flow State" primarily in rythm games, so we made a mash-up of a game like "_Crossy Road_" and a rythm game. I did all the programing for the game, and though it was tough, I felt the challenge of reliably timing things to a beat was really interesting to solve. The game is up on [itch.io](https://gdmbenedict.itch.io/when-life-gives-you-lemmings) and is available to be player in browser if you want to check it out.

![image](https://github.com/user-attachments/assets/8c767d92-a08c-4c46-bc7d-6c43f41b6ce4)

#### Global GameJam-2024: Dad-Sim 1997:
In the Global GameJam-2024 I took part in a team effort of some NSCC students and a Dalhousie student to create a game with the prompt "Make me Laugh". Our group decided to make a goofy physics based collection of minigames, in the style of a game like "Octodad". The I contributed programming for the player controls, some of the mini-games, as well as the UI and menus for the game. Unfortunetly, we were not able to complete our vision for the game by the end of the Jam, but I've gotten together with some members of the teams to fix some issues with the game and release it on itch.io. In the meantime, what we were able to complete by the GameJam deadline can be found on the official Global gameJam website [here](https://globalgamejam.org/games/2024/dad-simulator-1997-4).

![DadSim](https://github.com/gdmbenedict/gdmbenedict/assets/97464794/fa5ef64c-2135-4e25-b4f0-c4ba37db35d9)

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
