classDiagram
    class Game {
        -Int id
        -List~Player~ players
        -Deck deck
        -DiscardPile discardPile
        -Int currentPlayerIndex
        -Int extraTurns
        -GameState state
        +startGame()
        +makeTurn(playerId, actions)
        +applyCardEffect(card, target)
        +endTurn()
        +eliminatePlayer(player)
    }
    class Player {
        -Int id
        -String name
        -MutableList~Card~ hand
        -Boolean isAlive
        +drawCard(deck)
        +playCard(card, combination)
        +stealCardFrom(player, specific, cardName)
    }
    class Card {
        -CardType type
        -String name
    }
    class Deck {
        -Stack~Card~ cards
        -DiscardPile discardPile
        +shuffle()
        +draw() Card
        +peekTop(amount) List~Card~
        +insertCardAt(position, card)
    }
    class DiscardPile {
        -MutableList~Card~ cards
        +add(card)
        +takeAnyCard() Card
    }
    class Combination {
        -List~Card~ cards
        +isValid() Boolean
        +getEffect() CombinationEffect
    }
    class RuleValidator {
        <<interface>>
        +canPlayCards(player, cards, isCombination) Boolean
        +canDrawCard(player) Boolean
        +canPlayNope(currentlyResolving) Boolean
    }
    class ExplodingKittensValidator {
        .. implements RuleValidator ..
    }
    class Action {
        -ActionType type
        -Player player
        -List~Card~ cards
        -Player targetPlayer
        -String chosenCardName
    }
    class GameService {
        +createGame(playerNames) Game
        +processTurn(gameId, actions) TurnResult
        +getValidActions(gameId, playerId) List~Action~
    }
    class HistoryService {
        +saveGame(game, turnLog)
        +getGameHistory(playerId) List~GameRecord~
    }
    class StatisticsService {
        +updateStats(game)
        +getPlayerStats(playerId) PlayerStats
    }
    Game "1" --> "*" Player
    Game "1" --> "1" Deck
    Game "1" --> "1" DiscardPile
    Player "1" --> "*" Card : hand
    Deck --> DiscardPile
    GameService --> Game
    GameService --> ExplodingKittensValidator
    GameService --> HistoryService
    GameService --> StatisticsService