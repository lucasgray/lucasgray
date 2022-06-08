---
name: Advent of the Reaper Entry Three
description: States
layout: default
date: 2022-06-01
---

## State of the Adventinalia

At this point in my Unity learning path, I feel decently comfortable designing small games. The VR arcade, Eddie's tour. Until now I haven't needed anything too complex. I had to take a step back with the main Advent battle scene.

Think about what the tactical RPG needs to do.

1. Choose a turn
2. Pause and wait for the character to select a character
3. If a character is selected, display movement
4. once movement is chosen, pop up a menu allowing confirmation (among other things)
5. Ask if they'd like to attack, if so, which cell to attack?
6. Finally, display the attack vignette to resolve combat and return to #1/#2

That's for the player's turn! If it's the AIs turn, we walk through a subset of those items on a loop before returning input control back to the player. Also, we need to allow a player to roll back to a previous step (cancel selection).

I did some searching and arrived at a solution similar to a few I've seen, hoping to strike the balance between hacky and overly abstract.

![state file hierarchy](/assets/advent/state-file-hierarchy.png)

I decided to introduce the idea of `State`s. There is an overall `StateMachine` that keeps track of which states are active and which are overlaid by what (via a `Priority` field).

On `StateMachine` for example we have the method

```
public State PriorityActiveState()
{
    return ActiveStates().MaxBy(a => a.Priority).First();
}
```

Which is used often to select the active state (for example, other decoupled UI components may hide themselves if the active state is the battle vignette).

`GameState` holds the most important state for the battle map - the Set of board pieces, the map tile information, who's turn it is and how many turns has elapsed, that sort of thing. It is in charge of keeping track of turns. It also holds an instance to the `VisualNovelEngine` which is a good topic for another post :)

Finally, the majority of the rest of the `State`s in the folder implement the following interface / class combination - 

```
namespace States
{
    public interface State
    {
        int Priority { get; }

        bool Active { get; set; }
        List<State> IsOverlaidBy { get; set; }
        
        void HandlePrimaryAction();
        void HandleSecondaryAction();
        void HandleMousePosition(Vector2 newPos);

        //TODO left/right/up/down
    }
    
    public interface State<in T> : State
    {
        void Activate(T t);
        void Deactivate();
    }


    public abstract class BaseState<T> : MonoBehaviour, State<T> where T : class
    {
        public T DisplayProps { get; private set; }

        public virtual void Activate(T t)
        {
            DisplayProps = t;
            Active = true;
        }

        public virtual void Deactivate()
        {
            DisplayProps = null;
            Active = false;
        }

        public abstract int Priority { get; }
        public abstract bool Active { get; set; }
        public abstract List<State> IsOverlaidBy { get; set; }
        public abstract void HandlePrimaryAction();
        public abstract void HandleSecondaryAction();
        public abstract void HandleMousePosition(Vector2 newPos);
    }
}
```

The generic `T` exists so that when a `State` is `Activate`d, any typed properties can be passed in to be used for the rest of the lifecycle of the `State`. Sure it could be more tightly coupled to `GameState` for example but this let's them exist more independently.

The big idea here - See the `Handle*` methods. This allows a state - critically - to be in charge of capturing user input. The priority active state should be the one to capture and interpret input. For now, we're just focused on keyboard and mouse, but later on this can be used to implement left/right/up/down with a controller or phone (buttons on display).

As a simple example, here's how the Options menu was implemented (tied to pressing escape) - 

![options menu](/assets/advent/options-menu.png)

```
namespace States
{
    public class OptionsMenu : MonoBehaviour, State
    {
        public int Priority => 100;
        public bool Active { get; set; }
        public List<State> IsOverlaidBy { get; set; }

        public GameObject menu;

        public Sneezeguard sneezeguard;
        
        public void HandlePrimaryAction()
        {
            // do nothing
        }

        public void ShowMenu()
        {
            menu.SetActive(true);
            sneezeguard.Show();
        }

        public void HideMenu()
        {
            menu.SetActive(false);
            sneezeguard.Hide();
        }
        
        public void HandleSecondaryAction()
        {
            // do nothing
        }

        public void HandleMousePosition(Vector2 newPos)
        {
            // do nothing
        }
    }
}
```

And one more simple one - the `State` that pops up the battle vignette! When the vignette runs, we don't let the player interact until it's over, so handling input is really easy :) This time however, we actually use the generic.

![battle vignette](/assets/advent/battle-vignette.png)

```
namespace States
{
    public class AttackDisplayProps
    {
        public BoardPiece Ally { get; set; }
        public BoardPiece Enemy { get; set; }
    }
    
    public class BoardPieceAttackState : BaseState<AttackDisplayProps>
    {
        public override int Priority => 1;
        public override bool Active { get; set; }
        public override List<State> IsOverlaidBy { get; set; }

        public AttackFlourish attackFlourish;
        
        public StateMachine stateMachine;

        public override void Activate(AttackDisplayProps t)
        {
            base.Activate(t);
            
            StartCoroutine(DoAttack(t.Ally, t.Enemy));
        }

        public IEnumerator DoAttack(BoardPiece ally, BoardPiece enemy)
        {
            var instance = Instantiate(attackFlourish, attackFlourish.transform.parent, true);
            
            yield return instance.AttackAnimation();
            stateMachine.RequestDeactivateState(this);
            Destroy(instance.gameObject);
            ally.TakeTurn();
        }
        
        public override void HandlePrimaryAction()
        {
            // do nothing
        }

        public override void HandleSecondaryAction()
        {
            // do nothing
        }

        public override void HandleMousePosition(Vector2 newPos)
        {
            // do nothing
        }
    }
}
```