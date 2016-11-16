##*Introduction to WebGL*

-----
[**Visualize page (1)**](https://rawgit.com/Modelisation5ETI/WebGL_Intro/master/index.html) or [**Visualize page (2)**](http://htmlpreview.github.io/?https://github.com/Modelisation5ETI/WebGL_Intro/master/index.html)

-----

-----
**Implementation**

-----

**Terrain :**

Babylon.js permet nativement de créer un terrain à partir d'une carte de hauteurs :

 ```javascript   
  BABYLON.Mesh.CreateGroundFromHeightMap( "ground", // Label
    "data/worldHeightMap.jpg", // Heightmap
    200, 200,                  // largeur, longueur
    250,                       // Nombre de subdivisions
    0, 10,                     // Hauteur min, Hauteur max
    scene,                     // Scene
    false,                     // Mise à jour
    function()                 // Ajoute un imposteur pour les collisions
    {
      ground.setPhysicsState(BABYLON.PhysicsEngine.HeightmapImpostor, { mass: 0 });
    } );
```
  On note l'ajout de l'imposteur nécessaire à la collision grâce à la fonction passé en paramètre. Elle est appelée lors de la génération du terrain, afin d'ajouter à celui-ci l'imposteur correspondant à la carte de hauteurs. L'utilisation d'imposteurs permet de représenter des maillages simplifier afin de faciliter la détection de collision.

  Afin de pouvoir visualiser le projet sous Firefox, nous étions contraints d'utiliser une longueur inférieure ou égale à la largeur. En effet, il semblerait que la façon dont Firefox charge les textures peut devenir très couteuse et donner lieu à des erreurs :

>Error: WebGL: texImage2D: Chosen format/type incured an expensive reformat: 0x1908/0x1401

Certains comportement similaires pour d'autres projets sont expliqués [ici](https://github.com/mrdoob/three.js/issues/9109#issuecomment-254076793).

**Camera :**
Create terrain from heightmap.

**Moteur Physique**

