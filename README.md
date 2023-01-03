# Actually adding structures

(I will add a Step 0 at some point explaining how to use structure blocks and putting the structure files in the correct format)

### Step 1: Making a structure set file

[This website](https://misode.github.io/worldgen/structure-set/) is extremely helpful for generating worldgen files, and in this part of the tutorial we'll use it to generate a structure set file.

A structure set file is used for two things:

1. Defining what structures should be placed
2. How the structures should be spread out in the world

Let's use the structure set file for ocean ruins as an example:

```
{
  "structures": [
    {
      "structure": "minecraft:ocean_ruin_cold",
      "weight": 1
    },
    {
      "structure": "minecraft:ocean_ruin_warm",
      "weight": 1
    }
  ],
  "placement": {
    "type": "minecraft:random_spread",
    "salt": 14357621,
    "spacing": 20,
    "separation": 8
  }
}
```

The `structures` list tells the game to place the `ocean_ruin_cold` and the `ocean_ruin_warm` structures as a group. This ensures they will not overlap. If the `ocean_ruin_cold` and the `ocean_ruin_warm` structures has different structure set files, something like this could happen:

*(Insert image of the border between an ocean and lukewarm ocean biome with the two ocean ruin types colliding)*

By having both `ocean_ruin` structures in the same structure set, they won't be able to generate like this.

Now that you understand how the `structures` list works, let's actually start adding stuff to it. Press the `+` button next to `Structures` on the web page I've linked above, and you should see this:

*(In the actual explanation this will be an image, but I'm doing this with Notepad++ so bear with me here)*
```
----------------------
    Structures
  Structure [   ] (!)
  Weight [ 1 ]
----------------------
```

The `structure` is the file location of the structure. Because we're doing a simple structure, you can just do (namespace):(your_structure_name). The structure's `weight` is only important if you have multiple structures in the set that can generate in the same biomes, so for now you can ignore it.

The `placement` object tells the game how to spread out the structure across the world. There's currently two `type` options as of 1.19: `random_spread` and `concentric_rings`. The `random_spread` type, as the name suggests, randomly spreads out the structure. The `concentric_rings` type, on the other hand, places a predefined amount of structures in multiple different "rings", centered at the world origin. 

*(Insert image comparing `random_spread` and `concentric_rings` placement)*

All structures except the stronghold uses the `random_spread` type, so this tutorial will continue with explaining that. If you wish to learn more about the `concentric_rings` placement type, check the worldgen documention for it on (website).

The `random_spread` placement type has three additional fields: `spacing`, `separation`, and `salt`. 

*(Please note: While there are 5 extra fields to the `random_spread` placement type, most only are there for vanilla structures and shouldn't be used by custom structure generation. If you really want to know what all the values do, read the worldgen documention on it on (website).)*

`spacing` is generally the average distance in chunks the structures in the structure set have from each other. `separation` is the absolute minimum distance in chunks between structures in the structure set can be from each other (and must be smaller than `spacing`). Here's a way to visalize this:

This example uses a `spacing` of 8, and a `separation` of 4.
```
- - - - - - - -
- - - - - - - -
- - + + + + - -
- - + + + + - -
- - + + + + - -
- - + + + + - -
- - - - - - - -
- - - - - - - -
``` 

Structures are placed based on a grid of chunks. Spacing defines the size of the grid. A spacing of 8 means the game will attempt one structure placement from the set per 8x8 chunk area.

A larger `spacing` number means the average distance between structures will be higher. If you want a frame of reference:
- Shipwrecks have a spacing of 24
- Villages have a spacing of 34
- Ruined Portals have a spacing of 40


Separation defines the number of "padding" chunks. A spacing of 4 puts 2 chunks on each side of the chunk area that no structures in the set can generate in. This prevents structures in the set generating too close to each other. Most structures have a `separation` of around 8.

For your structure, don't worry too much about choosing a perfect `spacing` and `separation` value; it will take trial and error to get right and there's no need to make it perfect right now.

`salt` is a random number that in combination with the world seed determines the randomness in where the structure is placed. Two structures sets having the same `salt` means they will generate structures in the same place. This is why every structure should have a unique `salt` that another structure mod likely won't use. 

misode.github.io has a handy feature for the `salt` in structure sets; a way to quickly randomize the salt. Just press that to get a random salt and you'll be good to go.
