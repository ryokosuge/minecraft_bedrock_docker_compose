# Minecraft Bedrock サーバーを docker compose で構築してみる

メモ

https://medium.com/geekculture/setting-up-a-minecraft-bedrock-server-with-docker-a-comprehensive-guide-1e942f16a8c6

これ参考にまずは `docker compose up` してローカルで実行してみる

確認したいのはバージョン上げたり再起動した時の backup ファイルの残り方とか

まずはこれで立ち上げる

```yml
version: "3.4"
services:
  minecraft:
    image: itzg/minecraft-bedrock-server
    container_name: minecraft-server
    environment:
      EULA: "TRUE"
      GAMEMODE: survival
      DIFFICULTY: normal
      VERSION: 1.18
      LEVEL_SEED: minecraft
      LEVEL_NAME: My Minecraft
    ports:
      - 19132:19132/udp
    volumes:
      - ./data:/data
    stdin_open: true
    tty: true
```

```sh
minecraft_bedrock_docker_compose [ main][!][☁️  ryo.kosuge@gmail.com][⏱ 2s]
❯ docker compose up --build --remove-orphans
[+] Running 14/14
 ✔ minecraft 13 layers [⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿]      0B/0B      Pulled                                                                                                                                                                                             2.7s
   ✔ 3e440a704568 Already exists                                                                                                                                                                                                                          0.0s
   ✔ a418cc02daac Already exists                                                                                                                                                                                                                          0.0s
   ✔ 4f4fb700ef54 Already exists                                                                                                                                                                                                                          0.0s
   ✔ a0110c1ce5f9 Already exists                                                                                                                                                                                                                          0.0s
   ✔ 0163ae367d2f Already exists                                                                                                                                                                                                                          0.0s
   ✔ edff3107c244 Already exists                                                                                                                                                                                                                          0.0s
   ✔ fe365b7001e8 Already exists                                                                                                                                                                                                                          0.0s
   ✔ ec5046797b0b Already exists                                                                                                                                                                                                                          0.0s
   ✔ 00ecb79bdbd2 Already exists                                                                                                                                                                                                                          0.0s
   ✔ 5a7e34f0eea9 Already exists                                                                                                                                                                                                                          0.0s
   ✔ 82a194d1a45a Already exists                                                                                                                                                                                                                          0.0s
   ✔ ecdf0c2bcc7c Pull complete                                                                                                                                                                                                                           0.1s
   ✔ d5e9d046d66d Pull complete                                                                                                                                                                                                                           0.1s
[+] Running 2/2
 ✔ Network minecraft_bedrock_docker_compose_default  Created                                                                                                                                                                                              0.1s
 ✔ Container minecraft-server                        Created                                                                                                                                                                                              1.3s
Attaching to minecraft-server
minecraft-server  | DEBU[0000] Using /data to match uid and gid
minecraft-server  | DEBU[0000] Resolved UID=0 from match path
minecraft-server  | DEBU[0000] Resolved GID=0 from match path
minecraft-server  | Downloading Bedrock server version 1.18.33.02 ...
minecraft-server  | 2023/05/17 02:19:57 Setting difficulty to normal in server.properties
minecraft-server  | 2023/05/17 02:19:57 Setting level-name to My Minecraft in server.properties
minecraft-server  | 2023/05/17 02:19:57 Setting level-seed to minecraft in server.properties
minecraft-server  | Starting Bedrock server...
minecraft-server  | NO LOG FILE! - setting up server logging...
minecraft-server  | [2023-05-17 02:19:58:313 INFO] Starting Server
minecraft-server  | [2023-05-17 02:19:58:313 INFO] Version 1.18.33.02
minecraft-server  | [2023-05-17 02:19:58:313 INFO] Session ID c789f392-a412-45af-9333-abe973a67041
minecraft-server  | [2023-05-17 02:19:58:316 INFO] Level Name: My Minecraft
minecraft-server  | [2023-05-17 02:19:58:322 INFO] Game mode: 0 Survival
minecraft-server  | [2023-05-17 02:19:58:322 INFO] Difficulty: 2 NORMAL
minecraft-server  | [2023-05-17 02:19:59:568 INFO] opening worlds/My Minecraft/db
minecraft-server  | [2023-05-17 02:20:02:045 INFO] IPv4 supported, port: 19132
minecraft-server  | [2023-05-17 02:20:02:045 INFO] IPv6 not supported
minecraft-server  | [2023-05-17 02:20:03:761 INFO] Server started.
minecraft-server  | [2023-05-17 02:20:03:800 INFO] IPv4 supported, port: 38744
minecraft-server  | [2023-05-17 02:20:03:800 INFO] IPv6 not supported
```

`data` ディレクトリにファイルがたくさんできてた

```sh
minecraft_bedrock_docker_compose [ main][!?][☁️  ryo.kosuge@gmail.com]
❯ tree -L 1 data
data
├── Dedicated_Server.txt
├── allowlist.json
├── bedrock_server-1.18.33.02
├── bedrock_server_how_to.html
├── bedrock_server_symbols.debug
├── behavior_packs
├── definitions
├── development_behavior_packs
├── development_resource_packs
├── development_skin_packs
├── minecraftpe
├── permissions.json
├── premium_cache
├── release-notes.txt
├── resource_packs
├── server.properties
├── structures
├── treatments
├── valid_known_packs.json
├── world_templates
└── worlds

13 directories, 9 files
```

さて一度 stop させる

```sh
^CGracefully stopping... (press Ctrl+C again to force)
Aborting on container exit...
[+] Running 1/1
 ✔ Container minecraft-server  Stopped                                                                                    1.2s
canceled
```

バージョンあげて再チャレンジする

```yml
version: "3.4"
services:
  minecraft:
    image: itzg/minecraft-bedrock-server
    container_name: minecraft-server
    environment:
      EULA: "TRUE"
      GAMEMODE: survival
      DIFFICULTY: normal
      VERSION: LATEST
      LEVEL_SEED: minecraft
      LEVEL_NAME: My Minecraft
    ports:
      - 19132:19132/udp
    volumes:
      - ./data:/data
    stdin_open: true
    tty: true
```

VERSION を`LATEST`にして再度 up する

```sh
minecraft_bedrock_docker_compose [ main][!?][☁️  ryo.kosuge@gmail.com]
❯ docker compose up --build --remove-orphans
[+] Running 1/1
 ✔ Container minecraft-server  Recreated                                                                                  0.1s
Attaching to minecraft-server
minecraft-server  | DEBU[0000] Using /data to match uid and gid
minecraft-server  | DEBU[0000] Resolved UID=0 from match path
minecraft-server  | DEBU[0000] Resolved GID=0 from match path
minecraft-server  | Looking up latest version...
minecraft-server  | Downloading Bedrock server version 1.19.81.01 ...
minecraft-server  | Backing up behavior_packs into backup-pre-1.19.81.01
minecraft-server  | Backing up definitions into backup-pre-1.19.81.01
minecraft-server  | Backing up minecraftpe into backup-pre-1.19.81.01
minecraft-server  | Backing up resource_packs into backup-pre-1.19.81.01
minecraft-server  | Backing up structures into backup-pre-1.19.81.01
minecraft-server  | Backing up treatments into backup-pre-1.19.81.01
minecraft-server  | Backing up world_templates into backup-pre-1.19.81.01
minecraft-server  | Starting Bedrock server...
minecraft-server  | NO LOG FILE! - setting up server logging...
minecraft-server  | [2023-05-17 02:23:46:751 INFO] Starting Server
minecraft-server  | [2023-05-17 02:23:46:751 INFO] Version 1.19.81.01
minecraft-server  | [2023-05-17 02:23:46:751 INFO] Session ID 8eafa942-95ac-4136-afea-329585cc9cd3
minecraft-server  | [2023-05-17 02:23:46:751 INFO] Build ID 1.19.81
minecraft-server  | [2023-05-17 02:23:46:752 INFO] Branch r/19_u8
minecraft-server  | [2023-05-17 02:23:46:752 INFO] Commit ID 2606f6a1a1e3ecf907cb776d138dac765c5ba3ff
minecraft-server  | [2023-05-17 02:23:46:752 INFO] Configuration Publish
minecraft-server  | [2023-05-17 02:23:46:754 INFO] Level Name: My Minecraft
minecraft-server  | [2023-05-17 02:23:46:760 INFO] Game mode: 0 Survival
minecraft-server  | [2023-05-17 02:23:46:760 INFO] Difficulty: 2 NORMAL
minecraft-server  | [2023-05-17 02:23:48:649 INFO] opening worlds/My Minecraft/db
minecraft-server  | [2023-05-17 02:23:51:798 INFO] IPv4 supported, port: 19132: Used for gameplay and LAN discovery
minecraft-server  | [2023-05-17 02:23:51:798 INFO] IPv6 not supported
minecraft-server  | [2023-05-17 02:23:52:489 INFO] Server started.
minecraft-server  | [2023-05-17 02:23:52:489 INFO] ================ TELEMETRY MESSAGE ===================
minecraft-server  | [2023-05-17 02:23:52:489 INFO] Server Telemetry is currently not enabled.
minecraft-server  | [2023-05-17 02:23:52:489 INFO] Enabling this telemetry helps us improve the game.
minecraft-server  | [2023-05-17 02:23:52:489 INFO]
minecraft-server  | [2023-05-17 02:23:52:489 INFO] To enable this feature, add the line 'emit-server-telemetry=true'
minecraft-server  | [2023-05-17 02:23:52:489 INFO] to the server.properties file in the handheld/src-server directory
minecraft-server  | [2023-05-17 02:23:52:489 INFO] ======================================================
```

`backing up`してるっぽい。`data`の中覗いてみよう

```sh
❯ tree -L 1 data
data
├── Dedicated_Server.txt
├── allowlist.json
├── backup-pre-1.19.81.01
├── bedrock_server-1.19.81.01
├── bedrock_server_how_to.html
├── bedrock_server_symbols.debug
├── behavior_packs
├── config
├── definitions
├── development_behavior_packs
├── development_resource_packs
├── development_skin_packs
├── minecraftpe
├── permissions.json
├── premium_cache
├── release-notes.txt
├── resource_packs
├── server.properties
├── treatments
├── valid_known_packs.json
├── world_templates
└── worlds

14 directories, 9 files
```

全部 data の中に入ってるっぽい

だからこれを volumes で共有すれば良さそうな気がしてきたぞ

そういえば docker compose の volumes の意味しっかり理解はしてないから調べてみよう

https://amateur-engineer-blog.com/docer-compose-volumes/

なんとなく、なるほど感

適当に試してみるか

こんな感じにしてみた

```yml
version: "3.4"
services:
  minecraft:
    image: itzg/minecraft-bedrock-server
    container_name: minecraft-server
    environment:
      EULA: "TRUE"
      GAMEMODE: survival
      DIFFICULTY: normal
      VERSION: 1.18
      LEVEL_SEED: minecraft
      LEVEL_NAME: My Minecraft
    ports:
      - 19132:19132/udp
    volumes:
      - minecraft_data:/data
    stdin_open: true
    tty: true

volumes:
  minecraft_data:
```

`data`ディレクトリを消して、イメージも全部消して 1.18 で compose up してバージョン上げてみる

```sh
minecraft_bedrock_docker_compose [ main][!][☁️  ryo.kosuge@gmail.com]
❯ docker compose up --build --remove-orphans
[+] Running 14/14
 ✔ minecraft 13 layers [⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿]      0B/0B      Pulled                                                            13.4s
   ✔ 3e440a704568 Pull complete                                                                                           8.9s
   ✔ a418cc02daac Pull complete                                                                                           9.1s
   ✔ 4f4fb700ef54 Pull complete                                                                                           9.2s
   ✔ a0110c1ce5f9 Pull complete                                                                                           9.2s
   ✔ 0163ae367d2f Pull complete                                                                                           9.4s
   ✔ edff3107c244 Pull complete                                                                                           9.5s
   ✔ fe365b7001e8 Pull complete                                                                                           9.6s
   ✔ ec5046797b0b Pull complete                                                                                           9.7s
   ✔ 00ecb79bdbd2 Pull complete                                                                                           9.8s
   ✔ 5a7e34f0eea9 Pull complete                                                                                           9.9s
   ✔ 82a194d1a45a Pull complete                                                                                          10.0s
   ✔ ecdf0c2bcc7c Pull complete                                                                                          10.0s
   ✔ d5e9d046d66d Pull complete                                                                                          10.1s
[+] Running 2/2
 ✔ Volume "minecraft_bedrock_docker_compose_minecraft_data"  Created                                                      0.0s
 ✔ Container minecraft-server                                Created                                                      0.3s
Attaching to minecraft-server
minecraft-server  | DEBU[0000] Using /data to match uid and gid
minecraft-server  | DEBU[0000] Resolved UID=0 from match path
minecraft-server  | DEBU[0000] Resolved GID=0 from match path
minecraft-server  | Downloading Bedrock server version 1.18.33.02 ...
minecraft-server  | 2023/05/17 02:41:50 Setting difficulty to normal in server.properties
minecraft-server  | 2023/05/17 02:41:50 Setting level-name to My Minecraft in server.properties
minecraft-server  | 2023/05/17 02:41:50 Setting level-seed to minecraft in server.properties
minecraft-server  | Starting Bedrock server...
minecraft-server  | NO LOG FILE! - setting up server logging...
minecraft-server  | [2023-05-17 02:41:50:786 INFO] Starting Server
minecraft-server  | [2023-05-17 02:41:50:786 INFO] Version 1.18.33.02
minecraft-server  | [2023-05-17 02:41:50:786 INFO] Session ID 82c131c9-2cd5-4ed3-9a5f-0fb8055a9f19
minecraft-server  | [2023-05-17 02:41:50:786 INFO] Level Name: My Minecraft
minecraft-server  | [2023-05-17 02:41:50:789 INFO] Game mode: 0 Survival
minecraft-server  | [2023-05-17 02:41:50:789 INFO] Difficulty: 2 NORMAL
minecraft-server  | [2023-05-17 02:41:50:829 INFO] opening worlds/My Minecraft/db
minecraft-server  | [2023-05-17 02:41:51:509 INFO] IPv4 supported, port: 19132
minecraft-server  | [2023-05-17 02:41:51:509 INFO] IPv6 not supported
minecraft-server  | [2023-05-17 02:41:51:245 INFO] Server started.
minecraft-server  | [2023-05-17 02:41:51:263 INFO] IPv4 supported, port: 51562
minecraft-server  | [2023-05-17 02:41:51:263 INFO] IPv6 not supported
```

volume 用のディレクトリはいない

```sh
minecraft_bedrock_docker_compose [ main][!][☁️  ryo.kosuge@gmail.com]
❯ ls -la
total 48
drwxr-xr-x   6 ryokosuge  staff    192  5 17 11:34 ./
drwxr-xr-x  15 ryokosuge  staff    480  5 17 11:14 ../
drwxr-xr-x  12 ryokosuge  staff    384  5 17 11:14 .git/
-rw-r--r--   1 ryokosuge  staff      0  5 17 11:14 Makefile
-rw-r--r--   1 ryokosuge  staff  16860  5 17 11:42 README.md
-rw-r--r--   1 ryokosuge  staff    421  5 17 11:37 docker-compose.yml
```

volumes が出来上がっていた

```sh
minecraft_bedrock_docker_compose [ main][!][☁️  ryo.kosuge@gmail.com]
❯ docker volume ls
DRIVER    VOLUME NAME
local     minecraft_bedrock_docker_compose_minecraft_data

minecraft_bedrock_docker_compose [ main][!][☁️  ryo.kosuge@gmail.com]
❯ docker volume inspect minecraft_bedrock_docker_compose_minecraft_data
[
    {
        "CreatedAt": "2023-05-17T02:41:33Z",
        "Driver": "local",
        "Labels": {
            "com.docker.compose.project": "minecraft_bedrock_docker_compose",
            "com.docker.compose.version": "2.17.3",
            "com.docker.compose.volume": "minecraft_data"
        },
        "Mountpoint": "/var/lib/docker/volumes/minecraft_bedrock_docker_compose_minecraft_data/_data",
        "Name": "minecraft_bedrock_docker_compose_minecraft_data",
        "Options": null,
        "Scope": "local"
    }
]
```

なんか中身を見に行くのめんどくさいので省略

だいたい良さそう。

さて、バージョン上げてみよう

```yml
version: "3.4"
services:
  minecraft:
    image: itzg/minecraft-bedrock-server
    container_name: minecraft-server
    environment:
      EULA: "TRUE"
      GAMEMODE: survival
      DIFFICULTY: normal
      VERSION: LATEST
      LEVEL_SEED: minecraft
      LEVEL_NAME: My Minecraft
    ports:
      - 19132:19132/udp
    volumes:
      - minecraft_data:/data
    stdin_open: true
    tty: true

volumes:
  minecraft_data:
```

```sh
minecraft_bedrock_docker_compose [ main][!][☁️  ryo.kosuge@gmail.com][⏱ 16m20s]
❯ docker compose up --build --remove-orphans
[+] Running 1/1
 ✔ Container minecraft-server  Recreated                                                                                  0.3s
Attaching to minecraft-server
minecraft-server  | DEBU[0000] Using /data to match uid and gid
minecraft-server  | DEBU[0000] Resolved UID=0 from match path
minecraft-server  | DEBU[0000] Resolved GID=0 from match path
minecraft-server  | Looking up latest version...
minecraft-server  | Downloading Bedrock server version 1.19.81.01 ...
minecraft-server  | Backing up behavior_packs into backup-pre-1.19.81.01
minecraft-server  | Backing up definitions into backup-pre-1.19.81.01
minecraft-server  | Backing up minecraftpe into backup-pre-1.19.81.01
minecraft-server  | Backing up resource_packs into backup-pre-1.19.81.01
minecraft-server  | Backing up structures into backup-pre-1.19.81.01
minecraft-server  | Backing up treatments into backup-pre-1.19.81.01
minecraft-server  | Backing up world_templates into backup-pre-1.19.81.01
minecraft-server  | Starting Bedrock server...
minecraft-server  | NO LOG FILE! - setting up server logging...
minecraft-server  | [2023-05-17 03:03:16:368 INFO] Starting Server
minecraft-server  | [2023-05-17 03:03:16:368 INFO] Version 1.19.81.01
minecraft-server  | [2023-05-17 03:03:16:368 INFO] Session ID adb36aa6-9d17-4eea-81d1-a17707a50017
minecraft-server  | [2023-05-17 03:03:16:368 INFO] Build ID 1.19.81
minecraft-server  | [2023-05-17 03:03:16:368 INFO] Branch r/19_u8
minecraft-server  | [2023-05-17 03:03:16:368 INFO] Commit ID 2606f6a1a1e3ecf907cb776d138dac765c5ba3ff
minecraft-server  | [2023-05-17 03:03:16:368 INFO] Configuration Publish
minecraft-server  | [2023-05-17 03:03:16:368 INFO] Level Name: My Minecraft
minecraft-server  | [2023-05-17 03:03:16:371 INFO] Game mode: 0 Survival
minecraft-server  | [2023-05-17 03:03:16:371 INFO] Difficulty: 2 NORMAL
minecraft-server  | [2023-05-17 03:03:16:427 INFO] opening worlds/My Minecraft/db
minecraft-server  | [2023-05-17 03:03:16:237 INFO] IPv4 supported, port: 19132: Used for gameplay and LAN discovery
minecraft-server  | [2023-05-17 03:03:16:237 INFO] IPv6 not supported
minecraft-server  | [2023-05-17 03:03:17:629 INFO] Server started.
minecraft-server  | [2023-05-17 03:03:17:629 INFO] ================ TELEMETRY MESSAGE ===================
minecraft-server  | [2023-05-17 03:03:17:629 INFO] Server Telemetry is currently not enabled.
minecraft-server  | [2023-05-17 03:03:17:629 INFO] Enabling this telemetry helps us improve the game.
minecraft-server  | [2023-05-17 03:03:17:629 INFO]
minecraft-server  | [2023-05-17 03:03:17:629 INFO] To enable this feature, add the line 'emit-server-telemetry=true'
minecraft-server  | [2023-05-17 03:03:17:629 INFO] to the server.properties file in the handheld/src-server directory
minecraft-server  | [2023-05-17 03:03:17:629 INFO] ======================================================
```

動いてそう

あとはこれをどこかに載せてみてアクセスできるか確かめるかな

狙い目は AWS ECS かな

まずは手動で上げて、のちに CDK に移行しようかな
