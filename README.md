# RAIC 2019 Ladder
Ladder for the RAIC 2019

Collection of instructions and scripts how to setup your own ladder similar to RAIC 2019.
This project targeted specifically on th RAIC 2019 to be able to quickly test different strategies.

## Goals
- Ability to compare different strategies
- Ability to play multiple games with same players
- Ability to play games on different maps


##  Folder structure (in progress)

- `clients` Folder where all bots stored. This folder will be used for creation bot dockers 
- `maps` folder with maps. These maps will be used for running different tests.
- `runner` folder with application `aicup2019.exe`
- `games` folder with games. Each game will be stored in separate folder with JSON file with game definition, one JSON file with game file, one configuration file which was used for running game. 

## Using Docker for the bots

Building bot

    git clone https://github.com/kant2002/raic-2019
    cd raic-2019
    git checkout kant/lib
    cd clients\cpp
    docker build -t raic_cpp:dev .

Now you can run your bot. I use modified version of `Dockerfile` which fix Git shengians on Windows (.gitattribute anybody?). Also I use slightly different way to compile then planned by RAIC guys, but this is just POC, so it may improve.

Run ipconfig and look and the DockerNAT network.

    Ethernet adapter vEthernet (DockerNAT):
    ...
    IPv4 Address. . . . . . . . . . . : 10.0.75.1

You need this IP address.

Run game server

    .\aicup2019.exe --config .\both_bots.json --player-names bot_31001 bot_31002 --batch-mode --save-results match_results.json

Run bot inside container

    docker run -it raic_cpp:dev /bin/bash ./run.sh 10.0.75.1 31001

And run second instance of bot

    docker run -it raic_cpp:dev /bin/bash ./run.sh 10.0.75.1 31002

Once game server exits, it produce file `match_results.json` with recorded resutls. Game server executes sequentially, and docker exit after that
