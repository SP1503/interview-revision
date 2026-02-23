## Overview of system:
### What is TicTacToe ?
	- A board with N * N size with N - 1 players or 1 Bot can be used.
	- Each players will have their own symbol.
	- For Bot we can allocate a default symbol.
	- We need to provide provision to select the difficulty level of the bot.
	- Validate the symbols on player creation.
	- Any player can make their first move.
	- There can be any strategy for winning.
	- On each move we need to validate whether there is any winning symbol with current state of the board.
	- The board can end up in draw.
	- One any winner wins we can terminate the given game.
	- Need undo functionality, many times we can undo as we want.
## Requirement Gathering:
- Creation of N * N board.
- Creation of N - 1 players, Bot creation also needs to be done.
- Allow players to have their desired symbols representing them to play.
- Validation of symbols as per the players.
- Decision of who should start first.
- Choose the difficulty level of Bot.
- Creation of Different winning strategy.
- Check if the game is draw.
- Have a undo functionality, Any user can undo any other users moves for any number of times.
## Clarify Requirements:

## Use case Diagram: 
## Class Diagram:
![[tic-tac-toe.png]]
### Entities:
### Primitive Attributes:
### Non primitive Attributes:
### Interfaces:
### Design Patterns used:
- Strategy for winner checking,
- Strategy for BOT algorithm
- Builder for player, Game creation.
### Association Relationship:
- Move and Cell: Aggregation (Cell need not to be deleted when Move gets deleted)
- Symbol and Cell: Aggregation
- Game and Player: Aggregation
- Board and Game: Composition
- Move and Game: Composition

