# Maze Game – JavaFX Client (Part C)

A desktop maze game built with JavaFX and the MVVM pattern. The player guides a plane through a randomly generated maze to its target, and can ask the computer to show the shortest path.

This repository is the **GUI part** of a three-part university project (Advanced Topics in Programming). The maze algorithms and the client-server layer come from Part B, included here as a library (`lib/ATP-Project-PartB.jar`).

Built by Dekel Winkler and [partner's name].

## Features

- Generate a new maze of any size
- Move with the numeric keypad, including diagonal moves (`7`, `9`, `1`, `3`)
- Show the solution path on the maze
- Save a maze to a `.maze` file and load it back later
- Welcome screen and a victory screen when the target is reached

## How it works

When the app starts, it launches two local servers from Part B:

| Server | Port | Role |
|---|---|---|
| Maze generator | 5400 | Receives the maze size and returns a new maze, compressed |
| Maze solver | 5401 | Receives a maze and returns the solution path |

The GUI talks to both servers as a client over TCP sockets, sending and receiving serialized Java objects. Generated mazes arrive compressed and are decompressed on the client side.

The servers are configured in `src/main/resources/config.properties`:
```
threadPoolSize=4
mazeGeneratingAlgorithm=MyMazeGenerator
mazeSearchingAlgorithm=BestFirstSearch
```

### Architecture (MVVM)

```
View        →   ViewModel      →   Model          →   Part B servers
(FXML +         (MyViewModel)      (MyModel:          (generate / solve)
controllers)                       TCP client)
```

- **View** (`View/`): FXML screens and their controllers; handles drawing and keyboard input
- **ViewModel** (`ViewModel/`): connects the view to the model and exposes the game state
- **Model** (`Model/`): game logic, player movement, and communication with the servers

## Running

Requirements: JDK and Maven (the Maven wrapper is included).

```bash
./mvnw javafx:run
```

On Windows:
```bash
mvnw.cmd javafx:run
```

## Project structure

```
src/main/java/
  GuiMain.java        starts the servers and opens the welcome screen
  Model/              game logic and server communication
  ViewModel/          MVVM view model
  View/               JavaFX controllers
src/main/resources/
  *.fxml              screen layouts
  images/, icons/     graphics
  config.properties   server settings
lib/
  ATP-Project-PartB.jar   maze algorithms, servers and compression (Part B)
```
