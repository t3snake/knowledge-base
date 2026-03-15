---
tags:
 - game_dev
 - godot
---

# Godot 3D Nodes

[Godot Recipes](https://kidscancode.org/godot_recipes/4.x/3d/index.html)

## Attaching to Bone

Use #BoneAttachment3d node and set the bone to attach any notes as a child that moves along with the bone.

Eg: Used to mount main char on spider's head

## Physics interpolation

Sometimes, physics ticks and other functions can become out of sync and cause jitteryness, this is because for eg: input being handled separately like moving the mouse, is out of sync with move and slide, this causes look to be smooth but movement to cause jitteriness with look being out of sync while moving.
