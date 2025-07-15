# Veins Devcontainer Environment

This project provides a ready-to-use development environment for working with [Veins](http://veins.car2x.org/), a popular open-source vehicular network simulation framework. The environment is designed to be used with [Visual Studio Code Dev Containers](https://code.visualstudio.com/docs/remote/containers) and Docker, making it easy to set up and work with Veins, OMNeT++, and SUMO on any system.

---

## Project Structure

```
veins_devcontainer/
  ├── .devcontainer/
  │   ├── devcontainer.json
  │   ├── Dockerfile
  │   ├── install-omnetpp.sh
  │   ├── install-sumo.sh
  │   ├── install-misc.sh
  │   ├── install-docker-env.sh
  │   ├── docker-env.sh
  ├── install.sh
```

### Components

#### `veins_devcontainer/`

This is the main directory containing all scripts and configuration files needed to set up the development environment.

- **.devcontainer/**
  Contains all configuration and setup scripts for the development container.

  - **devcontainer.json**
    Main configuration file for VS Code Dev Containers. Specifies the container name, build instructions, required extensions, and shell settings. It ensures the environment is tailored for OMNeT++, SUMO, and Veins development, including:

    - Container name: "OMNeT++ and SUMO on Debian 12"
    - Builds from the provided Dockerfile
    - Installs recommended VS Code extensions (clangd, OMNeT++ NED syntax, LLDB debugger)
    - Sets up zsh as the default shell

  - **Dockerfile**
    Defines the base image and steps to install all required software on top of Debian 12. It:

    - Installs system dependencies
    - Runs scripts to install OMNeT++, SUMO, and other tools
    - Sets up the environment for development

  - **install-omnetpp.sh**
    Installs OMNeT++ (a discrete event simulator) and its dependencies. It:

    - Downloads and unpacks the specified OMNeT++ version
    - Installs required libraries and tools
    - Configures and builds OMNeT++ in both debug and release modes
    - Disables OpenSceneGraph (OSG) for compatibility in Docker

  - **install-sumo.sh**
    Installs SUMO (Simulation of Urban MObility) and its dependencies. It:

    - Downloads and unpacks the specified SUMO version
    - Installs required libraries and tools
    - Builds SUMO from source

  - **install-misc.sh**
    Installs additional useful tools for development and debugging, such as:

    - vim, zsh, valgrind, bear, clangd-13, lldb-13

  - **install-docker-env.sh**
    Ensures the Docker environment variables are set up for every shell session in the container by linking the `docker-env.sh` script to `/etc/profile.d/`.

  - **docker-env.sh**
    Script that sets up environment variables (e.g., `PATH` and `PYTHONPATH`) for OMNeT++ and SUMO, ensuring their binaries and Python packages are available in every shell session.

- **install.sh**
  A helper script to link the `.devcontainer` setup into an existing Veins installation. It:
  - Checks for the Veins source directory
  - Creates hard links for the devcontainer files in the Veins directory
  - Ensures the setup is not duplicated

---

## How to Use

1. **Clone this repository** (or copy the `veins_devcontainer` directory) into your Veins project root.
2. **Run `install.sh`** to set up the devcontainer configuration in your Veins directory. This will link all necessary files for the devcontainer setup.
3. **Open the project in VS Code** and use the "Reopen in Container" feature.
   VS Code will build the Docker image and set up the environment automatically.
4. **Start developing and simulating** with Veins, OMNeT++, and SUMO, all pre-installed and configured.

---

## Requirements

- [Docker](https://www.docker.com/)
- [Visual Studio Code](https://code.visualstudio.com/) with the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
- A Veins source directory (the setup expects to be linked into a Veins project)

---

## What is Veins?

[Veins](http://veins.car2x.org/) is an open-source framework for running vehicular network simulations. It couples the OMNeT++ network simulator with the SUMO road traffic simulator, allowing researchers and developers to simulate realistic vehicle movement and communication scenarios.

- **OMNeT++**: A modular, C++-based discrete event simulator for building network simulators.
- **SUMO**: An open-source, highly portable, microscopic and continuous road traffic simulation package.

This devcontainer setup ensures both OMNeT++ and SUMO are installed and configured to work together seamlessly, making it easy to start simulating with Veins.

---

## License

This project and its scripts are licensed under the GNU General Public License v2.0 or later. See individual files for details.

---

## Further Reading

- [Veins Documentation](http://veins.car2x.org/documentation/)
- [OMNeT++ Documentation](https://doc.omnetpp.org/)
- [SUMO Documentation](https://sumo.dlr.de/docs/)

---

If you have questions or need help, please refer to the official documentation or open an issue in your project repository.

---

## Important Commands

Below are some of the most useful commands you can run inside the container, along with explanations of what they do:

### General Devcontainer Commands

- **Rebuild the container**
  ```sh
  # In VS Code, open the Command Palette (Ctrl+Shift+P) and run:
  Dev Containers: Rebuild and Reopen in Container
  ```
  _Rebuilds the Docker image and restarts the development environment. Use this if you change the Dockerfile or setup scripts._

- **Open a terminal in the container**
  ```sh
  # In VS Code, open a new terminal (Ctrl+`)
  ```
  _Opens a shell inside the running container, where you can run all commands below._

### OMNeT++ Commands

- **Check OMNeT++ version**
  ```sh
  omnetpp --version
  ```
  _Displays the installed OMNeT++ version and confirms it is available._

- **Run OMNeT++ IDE (GUI)**
  ```sh
  omnetpp
  ```
  _Launches the OMNeT++ graphical IDE (if X11 forwarding or a GUI is available)._

- **Compile a simulation**
  ```sh
  make
  ```
  _Builds your OMNeT++ simulation project (run in your project directory)._

- **Run a simulation**
  ```sh
  opp_run -n .:../veins/src -l ../veins/src/veins
  ```
  _Runs a simulation using OMNeT++ (adjust paths as needed for your setup)._

### SUMO Commands

- **Check SUMO version**
  ```sh
  sumo --version
  ```
  _Displays the installed SUMO version and confirms it is available._

- **Run SUMO in command-line mode**
  ```sh
  sumo -c your_config_file.sumocfg
  ```
  _Runs a SUMO simulation using the specified configuration file._

- **Run SUMO with GUI**
  ```sh
  sumo-gui -c your_config_file.sumocfg
  ```
  _Launches the SUMO graphical user interface (if X11 forwarding or a GUI is available)._

### Veins/Devcontainer Setup Commands

- **Install devcontainer setup into Veins project**
  ```sh
  ./install.sh
  ```
  _Links the devcontainer configuration into your Veins project. Run this from the `veins_devcontainer` directory._
