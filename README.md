# Minecraft and Velocity Proxy Compose Stack

This is a basic Docker Compose stack for creating separate Survival, Creative and GregTech: New Horizons (GTNH) servers with a Velocity proxy.

The Survival and Creative servers use the [itzg/minecraft-server](https://github.com/itzg/docker-minecraft-server) image. The GTNH server is built using the included Dockerfile and will take a while to build and run.

Due to the version of Minecraft that GTNH is based on, it's not compatible with the Velocity proxy. GTNH is available directly on port 25567.
To make the best use of Velocity as a proxy, you will need to be able to modify the DNS on your local network or machine. If you are running this stack locally, you can modify the hosts file. The hosts configured in velocity are creative.minecraft and survival.minecraft on port 25566.

**This was made for Linux, if you are on Windows then it may 'just work' with Docker Desktop and WSL2**

## Security

**Do not expose these servers to the internet** unless you know what you are doing. The intention of this stack is for on a local home LAN where there is no access from the outside world.

## To Use

1. Create a file called ```forwarding.secret``` in the same directory as this README and docker-compose.yaml and add only a shared secret to the file. This is used for authentication between Velocity and the servers.
2. Create a directory on the Docker Compose host where the server files will be created and stored, this will be mapped into the containers.
3. Make yourself owner of the above dir if it's not in your home directory ```sudo chown -R $(id -u):$(id -g) <put_path_here>```
4. Add the path of the directory from the previous step to the ```MC_VOLUME_PATH``` in the ```.env``` file.
5. Bring the Docker Compose stack up to generate world and configuration files ```docker-compose up -d```
6. View container logs view the CLI, Docker desktop or the included [Dozzle container](http://localhost:8888). The default username/password is ```admin/password``` and can be changed in the users.yml file.
7. Once the servers are up and config files files are generated in the directory created earlier; bring the stack down using ```docker-compose down```.
8. Copy the paper-global.yml file into the config directory now created for each server eg ```/minecraft_network/creative/config/paper-global.yml```. Fill in the secret you created earlier and put in the ```forwarding.secret``` file. Alternatively just replace the proxies section with the changed config and secret to enable Velocity to connect.
9. Bring the stack back up with ```docker-compose up -d```
10. Connect to the creative and survival servers using DNS if you modified your hosts file ```creative.minecraft:25566``` or ```survival.minecraft:25566```, and for GTNH ```localhost:25567```.


## HowTo remove the Velocity proxy

If you don't want the hassle of the Velocity Proxy, you can just expose a different port for each container.

1. Remove or comment out the proxy configuration
2. Change the ```ONLINE_MODE``` to ```TRUE``` for both the survival and creative servers
  - ```yaml
      ONLINE_MODE: "TRUE"
    ```
3. Add a port configuration for each server similar to below. The first number of the configuration must be unique and big, you can choose 25568 and 25569 to be safe. The second number must be 25565.
  - ```yaml
      ports:
      - "25568:25565"
    ```
4. Disable the Velocity proxy configuration for each server in the ```paper-global.yml``` file

## Plugins

The survival server will install the following plugins for server management. Configuration is left as an exercise for the user.

- [EssentialsX](https://modrinth.com/plugin/essentialsx)
- [LuckPerms Plugin](https://www.spigotmc.org/resources/luckperms.28140/) and [Website](https://luckperms.net/)
- [Vault](https://www.spigotmc.org/resources/vault.34315/)
- [WorldGuard](https://dev.bukkit.org/projects/worldguard)
- [WorldEdit](https://dev.bukkit.org/projects/worldedit)

## Mumble

A simple Mumble container is included to provide an easy way for users to communicate without using an external service. This is especially useful for those that won't or can't (young kids) use Discord.

You will need to download the Mumble client. The configuration is intentionally left minimal with no authentication.

- [Mumble Site](https://www.mumble.info/)
- [Mumble Docker Github](https://github.com/mumble-voip/mumble-docker)

## GTNH - Gregtech: New Horizons

[GTNH](https://www.gtnewhorizons.com/) is a expert mod pack and will be difficult for anyone just getting started with Minecraft.

*this modpack is for people who enjoy hard challenges, and are willing and able to commit serious amounts of time to them. At the beginning, this may mean hours of searching for the right ores. Later on you'll spend time building and rebuilding your technology infrastructure to handle the latest resource demand. By the end, you'll practically have degrees in Gregtech chemical and electrical engineering - even a mage needs a technological base to supply resources!* - [GTNH Wiki](https://wiki.gtnewhorizons.com/wiki/Beginner_Tips)

GTNH uses quite a bit of memory and takes a while to start. If you don't intent on playing it yet or ever then it will be best to remove or comment out this server from the ```docker-compose.yaml``` file.

If you are going to try GTNH, you will need a launcher like [Prism](https://prismlauncher.org/) and to read the [beginners tips](https://wiki.gtnewhorizons.com/wiki/Beginner_Tips) or watch a few [Youtube videos](https://www.youtube.com/playlist?list=PLXuMrF6IqkF3UHw5cDUyoR3pndJdVmkcp).