---
name: Advent of the Reaper Entry Two
description: Map Movement
layout: default
date: 2022-06-01
---

## How Did We Get Here?

I've been busy on Advent! Working on it off and on when I have spare nights and weekends, outside of the Eddie Bowles walking tour. I've been trying not to outpace the artwork - still plenty to do, but I'm happy with my progress and the codebase so far. I'll focus on map movement for this post.

## Map Movement

Here's a screenshot of where things stand - 

![cursor and arrow](/assets/advent/cursor-and-arrow.png)

When the player's turn starts, they can choose a character to move. Next we need to render the character's move distance. We do this by taking into consideration their movement range and simulating movement to as many cells as allowed. We basically run Djikstra's algorithm (I think? It's been a long time since my undergrad days...) to determine this distance. 

There are two challenges - 

1. A character should be able to pass through (but not stop at!) a cell where there is currently another friendly character.
2. A character cannot pass through or stop at walls (or other unpassable terrain).

First things first, we kick off by starting at a given `Cell` (a field on the `MonoBehavior` here).  We enqueue it with a 0 distance, check N/S/E/W, enqueueing if necessary, then keep going through our queue till it's exhausted. In hindsight this could have been recursive but maybe the queue is more gentle on memory. Yeah. That's why I did it. Totally.

```
public List<MoveCell> PathingFor(HashSet<Cell> movementBlockers,
            List<BoardPiece> allies, List<BoardPiece> enemies, int distance)
{
    var toCheck = new Queue<MoveCell>();
    var candidateCells = new List<MoveCell>();

    toCheck.Enqueue(new MoveCell {Distance = 0, Position = Cell});

    while (toCheck.Count > 0)
    {
        var cell = toCheck.Dequeue();

        if (cell.Distance > distance) continue;

        candidateCells.Add(cell);

        var northCell = new MoveCell
            {Distance = cell.Distance + 1, Position = cell.Position.North()};
        var southCell = new MoveCell
            {Distance = cell.Distance + 1, Position = cell.Position.South()};
        var westCell = new MoveCell
            {Distance = cell.Distance + 1, Position = cell.Position.West()};
        var eastCell = new MoveCell
            {Distance = cell.Distance + 1, Position = cell.Position.East()};

        MaybeEnqueue(northCell, toCheck, candidateCells, movementBlockers, enemies, allies);
        MaybeEnqueue(southCell, toCheck, candidateCells, movementBlockers, enemies, allies);
        MaybeEnqueue(eastCell, toCheck, candidateCells, movementBlockers, enemies, allies);
        MaybeEnqueue(westCell, toCheck, candidateCells, movementBlockers, enemies, allies);
    }

    Debug.Log($"Cells we've found: {candidateCells.Count}");

    return candidateCells;
}
```

`MaybeEnqueue` is decently commented so I don't have much to add. Logic is easy enough except for the one really tricky situation about allies.

```
/**
    * Idea here is to add to cells to check if it doesn't exist already (if it does exist already
    * we know it has a shorter distance in the queue). We also must not enqueue if it was already
    * in candidate cells.
    */
private void MaybeEnqueue(MoveCell cell, Queue<MoveCell> toCheck, List<MoveCell> candidateCells,
    HashSet<Cell> blockers, List<BoardPiece> enemies, List<BoardPiece> allies)
{
    // blocker piece? cant move here!
    if (blockers.Contains(cell.Position)) return;

    // enemy? cant move through!
    if (enemies.Select(p => p.Cell).Contains(cell.Position)) return;

    // ally? can move through but not land on
    // this one is tricky. we cant show this as a passable area if they can only go one more
    // unit after that. although, what if they do have more potential moves, but none pan out?
    // we need to see if there is passable terrain after this too!
    if (allies.Select(p => p.Cell).Except(new List<Cell> {Cell})
        .Contains(cell.Position))
    {
        if (!CanMoveThrough(cell, blockers, candidateCells))
        {
            return;
        }
    }

    if (candidateCells.Exists(c => Equals(c.Position, cell.Position))) return;

    if (toCheck.Any(c => Equals(c.Position, cell.Position))) return;

    toCheck.Enqueue(cell);
}
```

Finally here's the implementation of `CanMoveThrough`. Certainly not going to break land/speed records but I think the tradeoff for readability is worth it.

```
private bool CanMoveThrough(MoveCell cell, HashSet<Cell> blockers, List<MoveCell> candidateCells)
{
    // if we would land directly ON this cell, let's not advertise that its passable
    if (cell.Distance == Character.MoveRange) return false;

    // otherwise passable if any of N/S/E/W is passable (and also not doubling back on that path)
    var east = cell.Position.East();
    var west = cell.Position.West();
    var north = cell.Position.North();
    var south = cell.Position.South();

    var passableTiles = new List<Cell> {east, west, north, south}.Except(blockers).ToList();
    var cameFrom = candidateCells.Find(c => c.Distance == cell.Distance - 1);

    return passableTiles.Except(new List<Cell> {cameFrom.Position}).ToList().Count > 0;
}
```

Once we mark up the cells internally with their move distances, it's easy to determine how the character should path to reach their destination (and to render the arrow).

1. Start at the destination
2. Look at adjacent cells
3. Enqueue the adjacent cell with the smallest move distance (meaning we're getting closer to the current position)
4. Move current cell to this cell and repeat

```
public List<MoveCell> MakePath(MoveCell destination, List<MoveCell> moveCells)
{
    var walkPosition = destination;
    var path = new List<MoveCell>();

    while (!Equals(walkPosition.Position, Cell))
    {
        path.Add(walkPosition);

        var next = ShortestNext(walkPosition, moveCells);
        walkPosition = next;
    }

    path.Add(new MoveCell {Position = Cell, Distance = 0});

    path.Reverse();

    return path;
}

MoveCell ShortestNext(MoveCell walkPosition, List<MoveCell> moveCells)
{
    var east = moveCells.Find(c => Equals(c.Position, walkPosition.Position.East()));

    if (east != null && east.Distance < walkPosition.Distance) return east;

    var west = moveCells.Find(c => Equals(c.Position, walkPosition.Position.West()));

    if (west != null && west.Distance < walkPosition.Distance) return west;

    var north = moveCells.Find(c => Equals(c.Position, walkPosition.Position.North()));

    if (north != null && north.Distance < walkPosition.Distance) return north;

    var south = moveCells.Find(c => Equals(c.Position, walkPosition.Position.South()));

    if (south != null && south.Distance < walkPosition.Distance) return south;


    Debug.Log("Couldn't find a position to move to, something went horribly wrong");
    return null;
}
```

That's it! The meat and potatoes of pathfinding for a character.

## Interlude: Terrain Type

I've hooked up the idea of terrain types - forest should take more moves to pass through, a path should be easy to travel on, that sort of thing. It's surfaced into the game state code but not currently included in the algorithm. What I'll need to do is dynamically alter the amount of moves left based on the current cell's terrain type.

## Finale: AI

We can use much of this code to determine how the enemy moves, too!

Right now, the enemies are awfully simplistic. If they are within 3x the movement range of an attackable character, they move towards the closest character, otherwise, they wait in their current cell. This is somewhat like the original Fire Emblem but there are a few more features to implement. 

