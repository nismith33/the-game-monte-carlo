# the-game-monte-carlo
Monte Carlo simulation to determine the odds of winning "The Game" if all players play optimally without communication.

**How to play The Game**
"The Game" is a card game for at least one player (the instructions recommend no more than five people play at once). The deck consists of 200 cards, two of every number from 1 to 100. At the start, the 1 cards and the 100 cards are laid face up on the table. These four cards are the base cards for four piles. Those with 1 as a base card are ascending piles, while those with 100 as a base are descending piles. At this point, each of the players at the table are dealt six of the remaining cards, and some player rotation is established.

A player's turn consists of playing at least two of their cards by placing them on one of the four piles (the cards played can go onto separate piles). A card can only be played on a pile if it moves in the right direction; that is, if its numerical value is higher than that of the top card of an ascending pile, or if its numerical value is lower than the top card of a descending pile (note these inequalities are strict, so a 60 cannot be played on a 60 in any kind of pile). There is one exceptional case in which a card may be played in the "wrong" direction: if the numerical value of a card is exactly 10 lower than the top card of an ascending pile or is exactly 10 higher than the top card of a descending pile, then it may be played on such a pile. As an example, 34 can be played on 44 in an ascending pile or on 24 in a descending pile. After they have played all desired cards for a round, the player draws from the remaining draw pile to get their hand back up to six cards. After the draw pile is empty, players only need to play one of their cards per turn.

The game ends either when one player is incapable of playing enough cards on their turn, or when all players have empty hands. Players are allowed to communicate with one another, but may not explicitly state what cards they have in their hand.

**What is this program for?**
The Game is rather frustrating to play. In practice, I found there was very little actionable information that could be shared with the other players without basically giving away the contents of your hand, so in practice adding players seems to just decrease one's odds of winning.

On the topic of odds of winning, the design of the game (and the fact that it is listed as playable as a solitaire game) leads me to believe that, like solitaire, there could be unwinnable starting deck configurations. So, a natural question arises: if all players play optimally (optimal play defined later), what are the odds of winning? While there may be a nice mathematical way to calculate these odds exactly, my combinatorics and game theory knowledge is not deep enough to know where to start. Instead, I will just use Monte Carlo simulation to calculate the odds of winning.

There are some other properties I would like to test with these simulations. Common sense seems to suggest that:
 -Increasing the minimum number of cards that must be played per turn will decrease the win rate
 -Increasing the number of players will decrease the win rate
Using the Monte Carlo simulation I can see whether these are real effects, and if so, how substantial they are.

**What strategy will the computer players use**
Firstly, there will be no communication between any of the players in this simulation. I simply cannot think of any information they might communicate that does not give away the contents of their hand. Usually the players will simply play in a greedy manner, putting down the card that results in the least immediate movement forward in a pile. As an example, suppose a player's hand consists of 2,3,51,67,68,80, while the ascending piles read 14 and 20, and the descending piles read 62 and 81. In such a case, the best move is to play 80 onto the 81 descending pile, since it only moves that deck forward by one unit, while any other move would cost 10+ units.

Of course, we have to get a little fancy with the jumps in the "wrong" direction. The computer should prioritize playing in the "wrong" direction whenever it can. That's easy enough if the player just needs to apply a 60 to an ascending 70 pile, but it will involve some extra work to make the player recognize an opportunity to create a skip in their hand. For example, given:
Hand - 40,51,61,84,95,96
Ascending - 42, 50
Descending - 50, 60
The best move is to play the 61 followed immediately by the 51 both on the Ascending 50. Because this behavior is a little more complicated than the basic greedy strategy, I may take longer to get around to implementing it.
