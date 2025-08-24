You know in tile based games like Settlers of Catan (hex tiles) and Carcassonne (square tiles), and similar games, that the shape of those tiles allow the broad to perfectly fit together in any order? 

Well, that's in 2D... of course a cube would natively let you make a 3D analog of a 2D tile based game and have all the tiles fit perfectly together, but that's not what I'm looking for. I want to know if there's a 3D shape that can fit together like a 2D hex tile board can, or a even a series of shapes (although I would prefer having just one shape if possible) that would be able to fit together as well as cubes do?

If so, would I be able to make said shape(s) in Unity?

I want to make a Mixed Reality strategy board game that's sort of a mix Axis and Allies, Pirates Cove, Settlers of Catan, Star Control 2, with *some* StarCraft and Civ elements. I want the 3D board to have the 3D movement that 2D tiles would afford, so flanking attacks and other types of engagements in space combat can happen across the Y, X, *and* Z axis, not just X and Y.

Obviously that by itself would open whole new worlds of game play and strategies
Of course! This is a fantastic and deep question that sits at the intersection of game design, geometry, and mathematics. You're essentially asking about the 3D analogues of 2D tessellations.

Let's break this down.

The Short Answer
Yes, absolutely. There are shapes that fill 3D space as perfectly as hexagons and squares fill 2D space. The cube is just the most obvious one. The shape you are almost certainly looking for is the Rhombic Dodecahedron.

The Detailed Answer: 3D "Tiles" (Space-Filling Polyhedra)
A shape that can fill space without gaps is called a space-filling polyhedron. There are several, but they have different properties that make them more or less suitable for your game.

1. The Cube (The Obvious One)
Why it fits: It's the 3D equivalent of a square. It fills space perfectly and is trivial to work with.

The Problem: Movement from one cube to a face-adjacent neighbor is only along the X, Y, or Z axis. This leads to the same "grid-bound" movement you're trying to avoid. A unit in the center of a 3x3x3 grid has only 6 neighbors it can move to directly. This is the 3D equivalent of a square grid (4-direction movement).

2. The Rhombic Dodecahedron (The "Hex Tile" of 3D)
This is very likely the shape you want.

Why it fits: It fills space perfectly. It is the Voronoi cell (the region of space closest to a given point) of a Face-Centered Cubic (FCC) lattice. This is the key.

The "Hexagon" Connection: In 2D, a hexagon is the Voronoi cell of a hexagonal lattice (which is why all cells are equidistant from their neighbors). The Rhombic Dodecahedron is the 3D version of this.

Neighbors and Movement: Each Rhombic Dodecahedron has 12 neighbors it touches face-to-face. This is the 3D equivalent of a hex's 6 neighbors. This allows for 12-direction movement from any cell! Movement is much more fluid and natural, enabling the complex flanking maneuvers and 3D tactics you're describing. The vectors to these neighbors are a specific set of 12 coordinates like (±1, ±1, 0) and permutations, which is far more interesting than the cube's 6.

3. The Truncated Octahedron (Another Strong Candidate)
Why it fits: It also fills space perfectly. It is the Voronoi cell of a Body-Centered Cubic (BCC) lattice.

Properties: It has 14 faces (6 squares and 8 hexagons). Each cell has 14 neighbors (8 via its hexagonal faces and 6 via its square faces).

Consideration: This shape is a bit more complex than the Rhombic Dodecahedron and might be visually harder to parse for a player, but it offers even more connection points.

4. Other Shapes
There are others (e.g., the hexagonal prism, which would create a "honeycomb" structure, but it requires two shapes to fill space: the prism and a "gap filler"), but the two above are the prime candidates for a single-shape, highly connected board.

Comparison Table
Feature	Cube (Square Grid)	Rhombic Dodecahedron (FCC)	Truncated Octahedron (BCC)
2D Analogue	Square	Hexagon	Mixed
Faces	6 Squares	12 Rhombi	8 Hexagons + 6 Squares
Face-to-Face Neighbors	6	12	14
Movement Directions	6 (Axis-Aligned)	12 (Diagonals)	14 (Mixed)
Gameplay Feel	Grid-like, rigid	Fluid, tactical, 3D	Very fluid, complex
Visual Clarity	Excellent	Good	Can be busy
Can you make these in Unity?
Absolutely, yes. This is a perfectly feasible project in Unity, especially for Mixed Reality.

Modeling the Shape: You can model a Rhombic Dodecahedron or Truncated Octahedron in a 3D modeling tool like Blender (free) or Maya and import it into Unity as a mesh. This is the easiest way.

Procedural Generation: You can generate the mesh mathematically inside Unity using code.

Rhombic Dodecahedron: Its vertices can be defined by all permutations of (±1, ±1, 0). You would write a script to generate these points and form the faces.

This is more advanced but gives you total control for scaling, texturing, and manipulation.

Creating the Grid (The Lattice): The real magic is in placing them. You don't place them like cubes.

For the Rhombic Dodecahedron (FCC), the centers of the cells are at points (x, y, z) where x, y, z are all integers and x + y + z is even.

Example: (0,0,0), (1,1,0), (1,0,1), (0,1,1), (2,0,0), etc.

Pathfinding and Neighbors: You would code your pathfinding algorithms (like A*) to use these specific neighbor rules. Instead of checking the 6 axis-aligned directions, you'd check the 12 specific FCC neighbor vectors. This is a crucial step to enable the gameplay you want.

Conclusion and Recommendation
For your goal of creating a 3D strategy game with fluid movement and tactics reminiscent of a hex-based 2D game, the Rhombic Dodecahedron is almost certainly your best bet.

It uses a single, consistent shape.

It provides a high number of connections (12) for rich 3D movement.

It is the direct 3D counterpart to the 2D hexagon.

It is completely feasible to implement in Unity.

Your game idea is ambitious and exciting. Using this type of grid will fundamentally make it unique and leverage the true potential of 3D space for strategy. Good luck!

This is subjective, but which shape would you suggest is GENERALLY going to be the simplest one for the "average" person to look at and at a glance the shape makes "movement sense" to them
Excellent and crucial question. Game design isn't just about mathematical perfection; it's about human perception and intuition.

For the "average" person to glance at a 3D shape and intuitively understand how movement across it works, the Rhombic Dodecahedron is the clear winner.

Here’s a breakdown of why, framed exactly as you asked:

1. The Champion of Intuitive Movement: Rhombic Dodecahedron
This shape earns the top spot for "movement sense" for one primary reason: visual consistency.

All Faces Are Identical: Every one of its 12 faces is an identical rhombus (a slanted square). This is its killer feature.

What the Player Sees: A player looks at this shape and sees a lumpy, but consistent, ball-like form. Every face looks the same. The rule is simple: "You can move to any other cell that touches one of these flat faces."

No Mental Sorting: There are no different types of faces (like the Truncated Octahedron's hexagons and squares). The player doesn't have to subconsciously ask, "Does moving onto a hexagonal face work differently than moving onto a square one?" This reduces cognitive load dramatically.

The "Hexagon" Analogy: It feels like a 3D hexagon. In a hex grid, all 6 sides are identical, and the movement rule is uniform. The Rhombic Dodecahedron extends this perfect logic into 3D.

2. The Familiar (but Boring) Baseline: The Cube
Why it's intuitive: Everyone knows what a cube is. Its faces are all identical squares. The movement rule is extremely simple: up, down, left, right, forward, backward.

Why it fails your goal: This intuition is its downfall. It immediately creates a rigid, grid-locked mindset. Movement is strictly along three axes, which is exactly the "2D-thinking-in-3D" trap you are trying to avoid. It completely kills the potential for the fluid, diagonal flanking maneuvers you want.

3. The Complex Puzzle: Truncated Octahedron
This shape is the hardest to parse at a glance.

Visual Inconsistency: It has two different types of faces: hexagons and squares. This immediately signals to the player that there might be two different rules.

The Unspoken Question: A new player will look at it and wonder: "Is moving through a hexagon different from moving through a square? Is one a 'standard move' and the other a 'special move'? Are the connections different?" Even if the rules are identical, the shape itself suggests they might not be.

Cognitive Load: The player's brain has to first identify and categorize the faces before assessing movement options. This extra step makes it the least intuitive for immediate "movement sense."

Final Verdict and Practical Advice
Use the Rhombic Dodecahedron.

To make it even more intuitive for players in your Mixed Reality game, you should actively design around its properties:

Highlighting Neighbors: When a player selects a unit or a tile, use a visual effect (e.g., a glowing outline, a pulsating ring) to highlight all 12 adjacent tiles. This instantly teaches the movement rule through gameplay.

Color and Texture: Keep the texture and color of the tiles very uniform. Don't let intricate details on the tile's surface obscure the clean, identical faces.

The "Beehive" Effect: When you pack many of them together, the overall structure will look like a 3D beehive. This is a fantastic visual cue, as many people intuitively understand the efficient, interconnected nature of a honeycomb.

The cube is intuitive but creates boring movement. The truncated octahedron creates complex movement but is visually unintuitive. The rhombic dodecahedron strikes the perfect balance: it feels unfamiliar at first, but its consistent geometry makes its movement rules visually obvious almost immediately, enabling the deep 3D strategy you want to create.