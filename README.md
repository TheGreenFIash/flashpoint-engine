# Flashpoint Engine
## Description
Flashpoint Engine is a 2D side-scrolling game engine developed by TheGreenFlash in Scratch. The engine intends to streamline the foundational programming process to provide game developers with powerful and efficient functionality. Development began in 2023, and the project is expected to be finished and released in 2025.

## Planned Features
### Particle Effects
Particle effects can be used to add animated details to the world to help bring it to life (e.g. flames, dust particles, rain, etc.). Particles in the effects are intended to use little memory and have brief lifespans of a second or two, allowing mamy to be displayed at once without significant impact to performance.
### Screen Shake Effects
Ten parameters define a screen shake effect, allowing a variety of screen shakes. The system is capable of producing short and violent jerks, sustained and gentle breezes, and everything in between. Additionally, screen shake effects can be layered to provide even more options to developers.
### Characters
A character consists of a rig and its associated graphics, which are each attached to a joint in the rig. The rig allows the displayed character to be articulated for animation purposes. Characters will be created with a GUI that is used to build rigs and assign costumes.
### Animations
Animations manipulate character rigs to create complex character movements (e.g. walk cycles, attack animations, etc.). The animation editor builds movement on cubic Bézier curves and supports forward and inverse kinematics, physics, and motion dependent on other joints. "Paramater joints" are independent, unattached joints in the character rig whose position may be updated by other parts of the program at runtime. They serve to make animations more adaptable, for example allowing the height and slope of the ground to change in a walk animation.
### Maps
The map editor will allow developers to build the gamespace. It is divided into the seven editors described below.
#### Graphics
Graphics make up the set pieces in the world. When not displayed, the graphics are held in four stacks, one for each axis. When a graphic goes offscreen, it is pushed onto the respective stack (in some cases it is sorted into the stack by how far offscreen it is). Since stacks are sorted by how far offscreen a graphic is along a particular axis, the program only needs to peek the top of the stack to determine if a graphic needs to be added, giving it constant O(1) time complexity. This innovation allows for large, complex worlds with no impact to performance.
#### Parallax Layers
Parallax layers provide a sense of depth to the world through layers of background that shift with the perspective of the camera. They are handled the same way as graphics, with each layer having four stacks. A single clone is used to handle each layer.
#### Collision Lines
Collision lines are used for physics interactions in the engine, primarily with objects and characters. The lines form an undirected graph, which is preprocessed by dividing the world into a grid and recording the collisions lines that intersect each grid cell. Because each cell can be accessed in constant O(1) time, this allows for a physics collision to be checked for a given position in constant O(1) time, assuming grid cells are small enough and collision lines are spread out as they would be in a typical world.
#### Pathfinding Graph
To navigate around the map, characters can use a directed graph. To mitigate lag when running the pathfinding algorithm, characters request path data between two points. Depending on the size of the request queue and the cost to find each path, requests may take several frames to process.
#### Lighting
An overall brightness effect can be applied to a section of the map outlined with a polygon. This effect is applied to map graphics as well as objects and characters in the effect area. It is not applied to particles or parallax layers.
#### Effect Groups
Effect groups act as an interface for world interactions. Map graphics are added to effect groups, which listen for given triggers. Effect triggers are typically an area on the map outlined by a polygon that can be activated by certain characters and objects. Triggers can also be operated manually in the code. The triggered effect is typically a change in position, orientation, transparency, or a similar effect. Effects have the format of two dinstinct states, with the trigger causing a transition between states.
#### Objects, Characters, and Particle Effects
Once the foundation of the map has been built, objects, characters, and particle effects can be added at specified locations on the map. This is typically the last step in the map design process.
### Renderer
A single renderer is used to display graphics. This modular approach allows rendering to be entirely disabled without affecting any other part of the engine. Graphics can be displayed using either stamping or clones, depending on developer preference, and the engine can switch between these modes at runtime if necessary.
