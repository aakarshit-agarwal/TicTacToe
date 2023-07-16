Design a system to play Tic-Tac-Tow for grid nxm

Functional Requirements:
    1. Player registration
    2. Player profile
    3. Create a room
    4. Add players to the room (exactly 2 for this game) [Random/Particular]
    5. Start the game
    6. Players will take turns
    7. Timer for player's turn
    8. Check for non-deterministic state
    9. Check for win/lose
    10. Add points to winner player profile
    11. Restart game
    12. Close game
    13. Game History


Non-functional Requirements:
    1. Low-latency
    2. Availablity
    3. 1000 games parallely
    4. 2000 active users

Scale:
    QPS: 2000 + 2000 web sockets


Services:
    1. UserManagementService
    2. GameManagementService

UserManagementService:
    Database:
        1. Users
            - UserId    [PK]
            - Email
            - Phone
            - UserName
        2. Profile
            - ProfileId [PK]
            - UserId    [FK]
            - Score
        3. Game
            - UserId    [PK1]
            - GameId    [PK2]
    APIs:
        1. CreateUser:
            [POST] game.com/users/
            Request: {
                userName: "Name",
                email: "Email",
                phone: "Phone"
            }
            Response: {
                userId: userId
            }
        3. DeleteUser:
            [DELETE] game.com/users/{userId}
        2. GetProfile:
            [GET] game.com/profiles/{profileId}
            Response: {
                score: 1234,
                playedGames: ["gameId1", "gameId2", ...]
            }
GameManagementService:
    Database:
        1. Player
            - PlayerId  [PK]
            - UserId    [FK]
        2. Game
            - GameId    [PK]
            - PlayerId1 [FK]
            - PlayerId2 [FK]
            - Status
            - BoardSize
            - Winner
            - StartTime
            - EndTime
            - BoardState
            - PlayerTurn
        3. GameLog
            - LogId     [PK][AI]
            - GameId    [FK]
            - Player    [FK]
            - ActionType
            - Action
    APIs:
        1. CreatePlayer:
            [POST] game.com/players/
            Request: {
                userId: "userId"
            }
            Response: {
                playerId: playerId
            }
        2. CreateGame:
            [POST] game.com/games/
            Request: {
                player1: player1Id,
                player2: player2Id,
                boardSize: 3x3
            }
        3. GetWinner:
            [GET] game.com/games/{gameId}/winner
            Reponse: {
                gameId: gameId,
                winner: userId
            }
        4. ListWinningGameForUser:
            [GET] game.com/games?wonBy=userId
            Response: {
                userId: userId,
                wonGames: [gameId1, gameId2, ...]
            }
        5. ListGameCreatedByUser:
            [GET] game.com/games?createdBy=userId
            Response: {
                userId: userId,
                createdGames: [gameId1, gameId2, ...]
            }
        6. GetGameStatus:
            [GET] game.com/games/{gameId}/status
            Response: {
                gameId: gameId,
                status: COMPLETED
            }
        7. Move:
            [SOCKET SEND]
            msg: {
                playerId: playerId,
                position: [x, y]
            }

Going Forward:
    API Versioning
    Advanced Features
    Infra Requirement
    Human Vs Computer Game
    