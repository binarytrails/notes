# Bake object texture to image texture

    - Select Cycles
    - Create a new image in the UV/image editor ( Alt + N or Image > New).
    - Add a texture node to the objects material(s) and select the new image.
    - With the texture node selected, press bake in Properties > Render settings > Bake

## seams

    # edit mod
    ctrl + r        make new seem parallel to edge loop

    f               connect two vertices

doing seams:

    edit mode > edge select > knife (k)
    duplicate edge (shift+d)
    separate by selection (p)
    object mod > select > convert to curve from mesh

    draw another curve
    select previous > curve options > geometry > bevel > object: another curve

