---
title: 'Architecting a Python Library'
date: 2022-07-24
draft: false
type: 'post'
tags:
  ['Blog Post', 'Project', 'Python', 'Pygame', 'Untitled Framework', 'Game Dev']
showTableOfContents: true
weight: 1
---

# Setting up a Todo List

I started out today by writing a simple todo list. I like to do this when starting out on a project to have a set of goals to achieve. These goals are set out in stone but they help me plan out what the framework will do and how to exactly architect it.

# Debug Menu

My first item on the todo list was to create a debug overlay. Now normally people will print or log a value to the console when debugging but you can't exactly do that with games. Say your game runs at 60 frames per second that's 60 print statements a second. This generated wall of text is very ugly and can be hard to read so instead I want you to be able to create text that appears when the debug overlay is active so you can see certain values realtime. If you have ever used Minecraft's f3 menu it's very similar.

I first set about creating a debugText class. This class will store the data for the text to be displayed on screen.

```
class debugText:
    """
    Instantiate a new line of debug text to be rendered.
    Text will be rendered each frame by calling debug.update.
    :param text: Text that will be displayed in debug overlay.
    :type text: str
    :param antialiasing: Will the text have antialiasing?
    :type antialiasing: bool
    :param color: The color of the text. ex: white is (255, 255, 255)
    :type color: tuple
    :param xy: The location of the text. ex: top left is (0, 0)
    :type xy: tuple
    :param font: What font to use to display the text. Needs to be from pygame.font.Font or pygame.font.SysFont
    """

    def __init__(self, text: str, antialiasing: bool, color: tuple, xy: tuple, font):
        self.text = text
        self.antialiasing = antialiasing
        self.color = color
        self.xy = xy
        self.font = font
        debug.append(self)
```

I'm going for a more functional oriented design so while I do have classes they are just used as data structures. This class is pretty straight forward; it just takes in the data pygame needs to draw onto the screen and for that reason, I decided to not do an abstract text class. At the very end of the init function I append the object to a list that will be used in the next step.

I next wrote a simple function that loops over the debug list which contains all the text and renders it then blits it to the screen.

```
def update(enabled, display):
    """
    If enabled will overlay screen with debug text.
    :param enabled: Is the debug overlay enabled?
    :type enabled: bool
    :param display: the pygame surface. You get this from pygame.display.set_mode()
    """

    if enabled:
        for line in debug:
            display.blit(
                line.font.render(line.text, line.antialiasing, line.color), line.xy
            )
```

# Rendering Pipeline

This task was pretty easy on the architecting side of things since I have used this system in a couple of games.
Essentially all elements you want drawn in game will inherit a base class called Entity.

```
import pygame

visibleSprites = pygame.sprite.Group()


class Entity(pygame.sprite.Sprite):
    def __init__(self, position: tuple, groups: list, image: str, size: tuple):
        super().__init__(groups)
        self.x = position[0]
        self.y = position[1]
        self.groups = groups
        self.imageRaw = pygame.image.load(image).convert_alpha()
        self.image = self.imageRaw
        self.rect = self.image.get_rect(topleft=position)
        self.size = size
```

Almost all of the code above is for pygame.
Pygame's sprite groups are essentially lists but you can call visibleSprites.draw() or other functions without having to define them.
The only parts of code that aren't for this are for the position and the size.

# Auto Resizing

Size is for autoresizing whenever you update the window size.

```
def resize(width: int, height: int):
    display = pygame.display.set_mode((width, height))

    for sprite in visibleSprites:
        aspectRatio = getSize((16, 9))
        sprite.image = pygame.transform.scale(
            sprite.imageRaw,
            (aspectRatio[0] * sprite.size[0], aspectRatio[1] * sprite.size[1]),
        )

    return display
```

This function essentially will resize the window but then it will go through every sprite in visibleSprites and resize them.

Note: The getSize call will be updated to support whatever aspect ratio you want

# Tween

This is a function that will give you the distance between point A and point B.

```
import math


def tween(a, b):
    if a <= 0 or b <= 0:
        raise ValueError("A and B must be greater than 0")

    return math.sqrt(a**2 + b**2)
```

As you can see I did this with Pythagorean Theorem which is much easier to implement than the distance formula.
There wasn't a specific reason why I did this, I just did it for fun.

# Scenes

Lastly for the largest part this week is scenes.
Scenes is how your game will be broken up.

For example if you're making a roguelike you would have a main menu scene and then a scene where the game would take place.

```
class Scene:
    def __init__(self, active, spriteGroups):
        self.active = active
        self.spriteGroups = spriteGroups
        scenes.append(self)

    def update(self):
        pass

    def load(self):
        raise LoadError(f"{self} is missing load instructions.")

    def unload(self):
        raise UnloadError(f"{self} is missing unload instructions.")


class LoadError(Exception):
    pass


class UnloadError(Exception):
    pass


def init(scene):
    scene.load()
    return scene


def switchScenes(activeScene, newScene):
    activeScene.unload()
    newScene.load()
    return newScene
```

This is not the final implementation.
I am removing loading and unloading if that's needed, it should be done custom but, since it won't be used in every scene the user can implement it.
Next post the scene system will recieve some updates to make it so much better this is just it at its crudest form.

Please, please if you use scenes put them in separate files than your main.py this is to help chunkate out parts of code.

# Wrapup

Sorry for not doing a screenshot Saturday this week there just aren't any visual things to really post but hopefully next week there will be screenshots to post.

That's all for this post.
Thanks for reading.
(ps. if you scroll to the bottom you can see my todo list.)

- [x] Move framework to library
- [x] Make it so user can create items to show in debug
- [x] Document debug
- [x] Window stuff
- [x] Start out architecting out abstract sprite stuff
- [ ] Start work on ui
- [x] Start work on tiles
- [x] Tween func
- [x] add error to tween func if param is <= 0
- [ ] Mask Collisions maybe?
- [ ] Refactor for launch
- [ ] Organize files
- [ ] Come up with name
- [ ] Documentation
- [ ] Animations
- [ ] Sound
- [x] Scenes
- [ ] Json loaded maps
- [ ] Fix scenes
