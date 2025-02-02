# Setting up a dev container for Rust

* Primary author: [Nick Kaplan](https://github.com/NickKaplan64)
* Reviewer: [Shriyans Sapkal](https://github.com/shrithebee1)

## Prerequisites

Before embarking on this journey, ensure you have completed the following steps:

1. Create a [Github](https://github.com) account
2. Download [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
3. Download [Visual Studio Code](https://code.visualstudio.com/)
4. Download [Docker Desktop](https://www.docker.com/products/docker-desktop)

## Initializing the repositories

1. Create the project directory and navigate into it:
    ```sh
    mkdir <Rust project name>
    cd <Rust project name>
    ```

2. Initialize the local git repository:
    ```sh
    git init
    ```

3. Create a README file and commit it:
    ```sh
    echo "# Rust Project" > README.md
    git add .
    git commit -m "Added README"
    ```

4. Create a new repository on GitHub and link it to your local repository:
    * Go to your GitHub account, click **[New Repository](https://github.com/new)**, and give it the same name as your project.
    * Copy the repository URL and link it to your local repository:
    ```sh
    git remote add origin https://github.com/<your-username>/<project-name>.git
    git branch -M main
    git push -u origin main
    ```

## Creating the dev container

1. Open project directory in VS Code and install the **Dev Containers** extension

2. Create a ```.devcontainer``` folder containing the file ```.devcontainer/devcontainer.json```

3. In this file, we will have a number of project configurations

    * **name**: This specifies a clear, descriptive label for your development container.
    * **image**: Defines the Docker image used to create the container. For this setup, we'll use the latest Rust environment image provided by Microsoft.
    * **customizations**: Allows you to configure additional features in VS Code, such as pre-installing useful extensions. Adding extensions here ensures other developers on your project have them installed in their dev containers automatically.
    * **postCreateCommand**: Specifies commands to run after the container is set up. In this case, we'll use it to install Rust dependencies.

    Your ```devcontainer.json``` file should look like this:
    ```json
    {
        "name": "<Rust Project Name>",
        "image": "mcr.microsoft.com/vscode/devcontainers/rust:latest",
        "customizations": {
            "vscode": {
            "settings": {},
            "extensions": ["rust-lang.rust-analyzer"]
            }
        },
        "postCreateCommand": "cargo new ./<Rust Project Name> --vcs none"

    }
    ```

!!! warning "```cargo new``` command" 
    Make sure to include the postCreateCommand as ```cargo new``` must be run on an empty directory. Since we have already created a README and Dev Container folder our directory would not be empty. Thus running this command allows us to initalize a Rust directory and keep our other files.

!!! note "```--vcs none``` flag"
    This flag is used to create a version control system, however since we already implemented a git repository it is unnecessary, hence ```none```.

4. Reopen the folder in the container
    * Press ++ctrl+shift+p++ or ++cmd+shift+p++, search for **Dev Container: Reopen in Container** and select it.
    * Wait for the container to launch

## Creating the Rust Project

Once the dev container is set up, you’re ready to create and build a Rust project.

1. Confirm that Rust is installed and up to date by running the following in a new VSCode terminal:
```sh
rustc --version
```

3. Navigate into ```./<Rust Project Name>/src/main.rs``` and update it to look like:
```rs
fn main() {
    println!("Hello COMP423");
}
```

!!! note ""
    Similar to languages such as Java or C, Rust uses a ```main()``` function as the entry point for your program.

!!! note ""
    The exclamation mark on ```println!``` is used to signify that this is a macro call. Essentially, it is Rust code that will write more Rust code before compilation. It is similar to a function in reducing the amount of code needed to be written. However, it offers more versatility in its definition and implementation in exchange for complexity.

4. Open a new terminal in VSCode and run the following:
```sh
cd ./<Rust Project Name>
cargo build
```

5. Run the built file directly
```sh
./target/debug/<Rust Project Name>
```

6. Alternatively, from your rust project folder, you can run the following in the terminal:
```sh
cargo run
```

!!! info "Difference between ```cargo build``` and ```cargo run```"
    Cargo build compiles and creates an executable file from your main file You can see this file in the ```target/debug``` directory. You can then run this file separately. Cargo run also compiles main and then immediately runs it without creating the executable file.

[^]: Steps regarding git and dev container initialization were taken from the [COMP 423](https://comp423-25s.github.io/resources/MkDocs/tutorial/#step-1-create-a-local-directory-and-initialize-git) site.

