# MBE03_BLOCK_VARIANTS

This example is a signpost which:

* doesn't occupy the entire 1x1x1m space,
* is made up of two pieces (the pole and a sign),
* uses a CUTOUT texture (with seethrough holes)
* has variants (can face in four directions, can be four different colours, and can be waterlogged (filled with water similar to vanilla sign or fence)

It will show you:

1. how to create a Block class with variants, and register it
1. how to define and register the model for rendering a block with variants
1. how to set the type of rendering for the block (SOLID, CUTOUT, TRANSLUCENT etc)
1. how to define and register the items corresponding to the different variants

The blocks will appear in the Blocks tab in the creative inventory.

The pieces you need to understand are located in:

* `StartupCommon` class
* `BlockVariants` class
* `resources\assets\minecraftbyexample\lang\en_US.lang` -- for the displayed name of the block and items
* `resources\assets\minecraftbyexample\blockstates\mbe03_block_variants_****` -- for the blockstate definition
* `resources\assets\minecraftbyexample\models\block\mbe03_block_variants_****` -- for the models used to render the differnt block types
* `resources\assets\minecraftbyexample\models\item\mbe03_block_variants_***` -- the models for rendering the different block variants as an item
* `resources\assets\minecraftbyexample\textures\block\mbe03_block_variants_sign_***` -- the textures used for the different sign colours

For background information on:

* blocks: see [http://greyminecraftcoder.blogspot.com/2020/02/blocks-1144.html](http://greyminecraftcoder.blogspot.com/2020/02/blocks-1144.html)
* rendering blocks: see [http://greyminecraftcoder.blogspot.com.au/p/list-of-topics.html](http://greyminecraftcoder.blogspot.com.au/p/list-of-topics.html) (the topics under the Block Rendering heading)
* Block Shapes (VoxelShapes): see [https://greyminecraftcoder.blogspot.com/2020/02/block-shapes-voxelshapes-1144.html](https://greyminecraftcoder.blogspot.com/2020/02/block-shapes-voxelshapes-1144.html)
* render types: see [http://greyminecraftcoder.blogspot.com/2014/12/block-rendering-18.html](http://greyminecraftcoder.blogspot.com/2014/12/block-rendering-18.html) - the method for specifying the render type is different, but the concepts of the different types (SOLID, CUTOUT etc are the same)

Useful vanilla classes to look at: `BedBlock`, `DoorBlock`, `ShulkerBoxBlock`

Miscellaneous notes:

* the mbe03_block_variants_model_shape defines the sign as "from": [4, 9, 8.01], "to": [12, 13, 8.01]. The reason for the 8.01 instead of 8 is to make sure that the sign renders just in front of the post. If you use 8, the post face and the sign face "fight" each other to be on top and this leads to weird striping / flickering at the overlap.
* the model_shape file also has the following for the sign--note the flipped north texture by using u values of 13-->4 for north instead of 4-->13
```
"faces": {
    "north": {"uv": [ 13, 3,  4, 7], "texture": "#sign"},
    "south": {"uv": [  4, 3, 13, 7], "texture": "#sign"}
}
```
* the sign textures have alpha channel information to make the signs pointy. You need a graphics editor which understands alpha channels in order to make textures like this. GIMP is a good free example.


## Common errors

"Missing Model", "Missing texture", etc:

See [http://greyminecraftcoder.blogspot.com.au/2015/03/troubleshooting-block-and-item-rendering.html](http://greyminecraftcoder.blogspot.com.au/2015/03/troubleshooting-block-and-item-rendering.html)

These are caused when you have specified a filename or path which is not correct, typically:

1. you've misspelled it
1. the upper/lower case doesn't match
1. you've forgotten the resource domain, eg `blockmodel` instead of `minecraftbyexample:blockmodel`
1. the folder structure of your assets folders is incorrect

The model has the wrong shape or is textured strangely:
1. your model file is wrong
1. you haven't set the RenderType correctly using RenderTypeLookup.setRenderLayer() (eg CUTOUT, SOLID, TRANSLUCENT etc)

Blocks or Items don't register (don't appear in the game at all):
1. You've registered your event handlers on the wrong bus (see MinecraftByExample class for more detail)
1. You're registering MyEventHandler.class on the event bus, but your event handler methods aren't static.
  1. or... You're registering myEventHandler instance on the event bus, but your event handler methods are static.
1. You haven't specified a tab for the item, eg .group(ItemGroup.BUILDING_BLOCKS);  // which inventory tab?