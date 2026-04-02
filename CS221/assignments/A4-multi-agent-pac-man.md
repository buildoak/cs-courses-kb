# CS221 Assignment 4: Multi-agent Pac-Man

## Problem Statement

This assignment takes the classic Pac-Man search problem and makes it honest: Pac-Man is not playing against one opponent, but against multiple ghosts with their own turns. The job is to build a decision agent that can search game trees with several adversaries, handle a deeper tree than the single-ghost version from lecture, and still make moves quickly enough to be useful.

The page breaks the work into three search models. First, derive and implement multi-agent minimax. Second, add alpha-beta pruning without changing the returned minimax values. Third, replace adversarial ghost behavior with random ghost behavior and implement expectimax. The extra-credit evaluation function asks for a better state scorer on top of that.

## Key Techniques

- **Turn-order aware tree search.** Pac-Man is agent 0 and the ghosts act in index order, so the recursion has to cycle through agents rather than alternate between just max and min.
- **Depth measured in full rounds.** A depth of 2 means every agent moves twice, not just Pac-Man twice.
- **Multiple min layers.** The minimax tree contains one min layer per ghost before control returns to Pac-Man.
- **Alpha-beta pruning across several minimizers.** The pruning logic has to survive more than one min layer per ply.
- **Expectation over random ghosts.** Expectimax swaps min for a probability-weighted average when ghost actions are modeled as uniform random choices.
- **State evaluation at leaves.** The leaf score comes from `self.evaluationFunction`, not from hand-written action heuristics.

## Implementation Walkthrough

1. **Start with the recurrence, not the code.** The assignment first asks for a piecewise definition of `V_minmax(s, d)` in terms of terminal states, utility, evaluation, legal actions, and successors. That makes the control flow explicit before you write recursion.
2. **Implement `MinimaxAgent` as a recursive game-state evaluator.** The starter code in `submission.py` asks you to work only with `GameState` objects. Pac-Man expands successors as max nodes, each ghost expands successors as min nodes, and the base case uses the evaluation function when the depth limit or a terminal state is reached.
3. **Treat alpha-beta as a structural optimization, not a new algorithm.** `AlphaBetaAgent` should return the same minimax values as `MinimaxAgent`. The only change is tracking alpha and beta bounds while traversing multiple ghost layers and pruning branches once the current bound is dominated.
4. **Switch the ghost model for `ExpectimaxAgent`.** The recurrence changes from minimum to expected value over legal ghost actions. That is the conceptual pivot of the assignment: the same game tree, but a different model of what the ghosts are doing.
5. **Use the validation numbers as guard rails.** The handout gives known minimax values for `minimaxClassic` and `mediumClassic`. Those numbers are the quickest way to catch a depth-counting bug or an incorrect agent index transition.
6. **Treat the evaluation function as a separate lever.** The extra-credit `betterEvaluationFunction` is where feature engineering matters: food distance, ghost distance, win/lose states, and score all need to be combined without making the agent paranoid or lazy.

## What You Learn

- Multi-agent search is just minimax with more bookkeeping, but the bookkeeping matters.
- Alpha-beta pruning only helps if the tree shape and action order are consistent.
- Expectimax is what you use when the world is not adversarial, only uncertain.
- A good evaluation function can rescue a shallow searcher, but it cannot fix a broken tree model.
- Search gets much easier once you stop pretending that all opponents behave the same way.

## Sources

- Assignment page: `https://raw.githubusercontent.com/stanford-cs221/autumn2019/gh-pages/assignments/pacman/index.html`
- Course home page: `https://raw.githubusercontent.com/stanford-cs221/autumn2019/gh-pages/index.html`
- Public repo: `https://github.com/stanford-cs221/autumn2019`
