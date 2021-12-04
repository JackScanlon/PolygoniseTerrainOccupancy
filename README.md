# Polygonising RBX Terrain occupancy
> **Note:** Although interpolation is implemented this will not produce a 1:1 replica of the terrain. This module is intended for topological/spatial queries.

Lua implementation of the Marching Cubes algorithm to polygonise the 3 dimensional occupancy grid returned from the *ReadVoxels* method.

## Usage
**Installation**
Download the main.lua file and insert into your environment as a module.

**Documentation**
Usage and arguments are as follows:
```lua
  local marching = require(main)
  --[[
    setIsolevel(float [0->1]):
      - Sets isolevel to determine threshold for isosurface
  ]]
  
  marching:setIsolevel(0)
  
  --[[
    queryTerrain(min [Vec3], max [Vec3]):
      - Takes two Vector3 arguments, one as the minimum boundary and the other as the maximum to query.
      - Querying areas larger than 256x256x256 will be chunkified.
      
    Note: Min/Max arguments will be expanded to a 4x4x4 grid when querying terrain occupancy.
  ]]
  
  marching:queryTerrain(Vector3.new(0, 0, 0), Vector3.new(128, 128, 128))
  
  --[[
    getResults():
      - Returns 1d list of dicts with the parameters of:
        [{
          position: centre of chunk [Vec3];
          vertices: verts of poly [1d array of Vec3];
          faces: index pointers to vertices [2d array; integer];
        }]
  ]]
  
  local res = marching:getResults()
  print(res)
```

Chaining is supported as follows:
```lua
  local min, max = Vector3.new(0, 0, 0), Vector3.new(128, 128, 128)
  local res = marching:setIsolevel(0):queryTerrain(min, max):getResults()
  print(res)
```
