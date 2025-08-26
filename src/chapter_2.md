# Chapter 2: Godot Beginnings

Us programmers have the important role of creating Game Objects, or "toys", for
the game designers to play with. In this tutorial we will make a simple toy and
demonstrate how to play with it in the Godot editor.

###### TODO: include video of simple godot node creation.

If you haven't already installed Godot, you can do that
[here](https://godotengine.org/download/windows/).

## 2.1: Creating a Godot Project

Opening Godot, you're greated with something like this:

![Godot landing page showing sample projects.](./godot-landing.png "Godot Landing")

Click "+ Create" in the top left corner. This opens a popup window with some
basic project configuration. Don't worry about any options other than the name
and the location to save your project (that is, if you know what you're doing
go ahead and set the graphics settings to your liking), but make sure that the
"Version Control Metadata" option is set to "Git". We need this for Godot to
automatically generate git metadata files (the equivalent of running `git init
.` as seen in [Chapter 1](./chapter_1.md)).

You're now greeted with an empty 3D scene like this:

![Godot editor landing with empty 3D space.](./godot-editor.png "Godot Editor")

What's nice about Godot is that it's less cluttered than a larger, proprietary
editor. Nonetheless, there is much to parse here.

Now is a good time to describe the basic object of Godot: the Node.

## 2.2: The Node

A node in Godot is an object that has up to one parent and can have many
children. All nodes live in a scene, which is an environment where the editor
and runtime update the nodes it contains. There exists at any time exactly one
"root node", which is the only node in the scene that does not have a parent.
Therefore:

- A scene is a tree of nodes.
- A node is an element of the "scene tree" that has attached properties.
- These properties are user-defined and based on the "type" of the node.
- With some caveats, any node can be a child or parent of any other node.

Nodes are used to separate components of an object's logic into units. For
example, a playable character might have the following nodes:

- A `CharacterBody2D`, something that's updated by the physics engine and
  optimized for "player-like" movements.
  - A `CollisionShape2D`, the child of the `CharacterBody2D`, which gives it a
    collision shape.
  - An `AnimatedSprite2D`, which is a node that gets rendered as a sequence of
    sprite textures.
  - A `RayCast2D`, which lets programmers detect when another in-game object is
    "colliding" with some field of view of the sprite.

Example sprites we'll make may have more nodes than this. One can also define
custom node types, which we'll cover more in [Chapter 3](./chapter_3.md) about
advanced scenes and signals.

## 2.3: Creating a Scene

The first scene we'll create will be modeled after Link from the original The
Legend of Zelda.

###### TODO: insert image or gif of Link here

Click on "2D Scene" in the left menu bar. You're then greeted by this:

![Godot empty 2D editor.](./godot-2d-editor.png "Godot 2D Editor")

The top portion of the left menu bar, named "Scene", shows a tree view of the
root node and all its descendants. We will refer to this portion as the "scene
tee" or the "tree view". The bottom portion, named "FileSystem", shows the
files (scene files, resources, assets, extensions, etc.) currently in the
project working directory. We will simply refer to this portion as the
"filesystem". Note that some hidden files are not shown immediately in the
editor, such as git metadata and some godot project files.

Start by clicking on the node in the scene tree and pressing `Ctrl+A` or
right-clicking on "Add Child Node...". You then see a popup like this:

![Godot create new node.](./godot-create-new-node.png "Gdoot Create New Node")

Get used to this popup as you'll see it frequently. In the search bar look up
"CharacterBody2D" and select that type. Hit enter. Now select the node that
just appeared and press `F2` or right-click on "Rename" and give it the name
"player".

###### TODO: talk about style guides for WGS. Why do we rename?

Now click on the "player" node in the scene tree and press `Ctrl+A` or
right-click on "Add Child Node..." again. This time, create a
`CollisionShape2D` and rename it to "player\_collider".

In the right menu bar of the editor lives a few submenus. The one currently
visible, named "Inspector", holds a bunch of data about the currently selected
node. As we're currently on "player\_collider", we'll see this:

![Godot inspector for collision shape.](./godot-inspector.md "Godot Inspector")

These parameters control many aspects of the node itself from its transform,
texture, render properties, etc. Here, click on the box that says "\<empty\>"
and select "New RectangleShape2D". You now see in the 2D editor a small
square box. This is the collider we just created. Notice how you can resize it
to your heart's desire. If you'd like finer control, click on the rectangle in
the inspector that now says "RectangleShape2D". A submenu should appear with
some parameters.

###### TODO: roadmap: add Animated Sprite, create script, add movement, demo.

Now click the "player" node in the scene tree and add a child node. Search for
"Sprite2D" in the node create menu and select it. Rename it to
"player\_sprite". Then navigate to the inspector and click on the box that says
"\<empty\>" as before. This time, click on the "New GradientTexture2D" option.
You now see seomthing like this:

![Godot after creating Sprite2D.](./godot-after-sprite.md "A Visible Sprite2D")

This is a temporary sprite we'll use for debugging. Later, we'll replace this
with an animated sprite that has changing textures in response to user input.
At this point we can save our progress using `Ctrl+S` or by navigating to
"Scene-\>Save Scene" in the top menu bar. Let's call it "link.tscn". You now
see the file in your filesystem in the bottom-left panel.

Now let's breathe life into the node. Right-click on the "player" node and
select "Attach Script...", then hit "Create". You now see a text editor instead
of the scene editor. To navigate between the two, use the menu items "2D" and
"Script" in the top-middle of the editor. Now paste this code into the editor:
```go
extends CharacterBody2D

const WALKING_SPEED: float = 120.0

func _ready() -> void:
	pass

func _physics_process(delta: float) -> void:
	velocity = Vector2.ZERO
	if (Input.is_key_pressed(KEY_W)):
		velocity.y = -WALKING_SPEED
	if (Input.is_key_pressed(KEY_S)):
		velocity.y = WALKING_SPEED
	if (Input.is_key_pressed(KEY_A)):
		velocity.x = -WALKING_SPEED
	if (Input.is_key_pressed(KEY_D)):
		velocity.x = WALKING_SPEED
	move_and_slide()
```

We'll do a line-by-line of this code later in this tutorial. For now, we should
be ready to run the project.

To run, press `F5` or click the play button in the top-right corner of the
editor and use WSAD to move. You now have a primitive but playable character!

![Godot primitive Link.](./godot-primitive-link.gif)
