##*Introduction to WebGL*

-----
[**Visualize page (1)**](https://rawgit.com/Modelisation5ETI/WebGL_Intro/master/index.html) or [**Visualize page (2)**](http://htmlpreview.github.io/?https://github.com/Modelisation5ETI/WebGL_Intro/master/index.html)

-----

-----
**Implementation**

-----
La scène est composée d'éléments disponibles nativement dans Babylon.js afin de mettre en évidence l'étendue des possibilités qu'offre WebGL. 

**Gestion de la physique :**

On utilise cannon.js pour la gestion des collision et de la physique. Un vecteur de gravité d'une valeur de -9.81 est appliqué continuellement selon l'axe y : 

```javascript
    var gravityVector = new BABYLON.Vector3( 0,-9.81, 0 );
    var physicsPlugin = new BABYLON.CannonJSPlugin();
    scene.enablePhysics( gravityVector, physicsPlugin );
```
La prise en charge de cannon.js par Babylon.js permet la gestion de la physique simplement en ajoutant des imposteurs aux maillages. Il est ensuite possible d'appliquer des forces à ces objets, par exemple lorsqu'une touche est activée, ou encore en résultat d'une collision. L'utilisation d'imposteurs permet de représenter des maillages simplifiés afin de faciliter la détection de collision.

**Creation de maillages simples :**

Des maillages simples tels que le cube, la sphère ou encore le cylindre sont disponibles :

```javascript  
  BABYLON.Mesh.CreateSphere( "sphere", //Label
                             10,       // Nombre de segments
                             10,       // Rayon
                             scene );  // Scène
```
La sphère constitue le personage principal de la scène et peut être déplacée à l'aide des touches directionnelles du clavier. Il est alors nécessaire d'assigner un imposteur à celle-ci :

```javascript  
  sphere.physicsImpostor = new BABYLON.PhysicsImpostor( sphere,
    BABYLON.PhysicsImpostor.SphereImpostor,     // Type d'imposteur
    { mass: 1, restitution: 0, friction: 0.1 }, // Paramètres physiques
    scene );                                    // Scène
```

Il est également possible d'ajouter un matériau au maillage. Dans notre cas, on créer un matériau standard auquel on applique une texture :
```javascript  
  var sphereMaterial = new BABYLON.StandardMaterial( "sphere", scene );
    sphereMaterial.diffuseTexture = new BABYLON.Texture( "data/earth.jpg", scene );
    sphere.material = sphereMaterial;
```

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
  On note l'ajout de l'imposteur nécessaire à la collision grâce à la fonction passé en paramètre. Elle est appelée lors de la génération du terrain, afin de générer un imposteur à partir de la carte de hauteurs.

  Pour de pouvoir visualiser le projet sous Firefox, nous étions contraints d'utiliser une longueur inférieure ou égale à la largeur. En effet, il semblerait que la façon dont Firefox charge les textures peut devenir très couteuse et donner lieu à des erreurs :

>Error: WebGL: texImage2D: Chosen format/type incured an expensive reformat: 0x1908/0x1401

Certains comportement similaires pour d'autres projets sont expliqués [ici](https://github.com/mrdoob/three.js/issues/9109#issuecomment-254076793).

**Lumière et Skybox :**

On ajoute une Skybox en créant un cube que l'on texture à l'aide de la méthode suivante :
 ```javascript  
BABYLON.CubeTexture( rootUrl, scene, extensions, noMipmap, files )
 ```
Cette fonction permet de charger les six textures de la skybox. 'rootURL' correspond à la racine du nom des textures. Les extensions par défaut sont :[_px.jpg, _py.jpg, _pz.jpg, _nx.jpg, _ny.jpg, _nz.jpg].

Un soleil est modélisé à l'aide d'un maillage sphère et d'une Spotlight. Une trajectoire circulaire est appliquée à l'ensemble afin de visualiser le changement des ombres au sein de la scène.

**Materiau océan :**

L'océan (qui peut être ajouté avec la touche 'w'), est constitué d'un plan auquel on applique un matériau d'effet d'eau. Ce matériau est disponible dans Babylon.js en incluant le script 'Babylon.js-master/materialsLibrary/dist/babylon.waterMaterial.js'.

