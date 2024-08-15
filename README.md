# ign-tag-ci-cd

___

## Prerequisite

Understand the process of creating docker containers using the [docker-image](https://github.com/design-group/ignition-docker).

This project assumes you have a local DG Traefik reverse proxy running, if not, the script will set one up or you can set one up using this repository [dg-traefik-proxy](https://github.com/design-group/dg-traefik-proxy)

All Design Group project repositories use the following automation tools:

- [pre-commit](https://pre-commit.com/)
- [pylint](https://pylint.org/)
- [markdownlint](https://github.com/markdownlint/markdownlint)
- [shellcheck](https://github.com/koalaman/shellcheck)
- [yamllint](https://github.com/adrienverge/yamllint)

Please confirm they are present in your environment before continuing.

___

## Setup

1. Follow [this guide from Github](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template) to create a new repository from the template.
1. Go the repository page on GitHub you created in the previous step and copy the clone link under Code-->HTTPS. Navigate to the folder where you want to have the code.

    ```sh
   git clone <clone link for HTTPS>
    ``` 

1. Run the initialization script from the root directory within a bash terminal in VS Code.

    ```sh
   ./scripts/initialize.sh
    ```

In a web browser, access the gateway at [http://ign-tag-ci-cd.localtest.me](http://ign-tag-ci-cd.localtest.me)

___

## Code Linting

The following tools are used to lint the code:

1. Python: [pylint](https://pylint.org/) 
   - The configuration file is located at `linting/.pylintrc`
2. Markdown: [markdownlint](https://github.com/igorshubovych/markdownlint-cli) 
   - The configuration file is located at `linting/.markdownlint.json` 

The linting is performed using the [pre-commit](https://pre-commit.com/) framework. 
The configuration file is located at `linting/.pre-commit-config.yaml`. 
The linting is performed on all files that are staged for commit. 

To run the linting manually, use the following command:

```sh
pre-commit run --all-files
```

___

## Gateway Config

The gateway configuration is stored in stripped gateway backups, that have all projects removed. In order to get these backups run the following command:

```sh
./scripts/download-gateway-backups.sh
```

The backups are stored in the `backups` directory, and if configured in the compose file will automatically restore when the container is initially created. 

If you are preparing this repository for another user, after configuration of the gateway is complete, run the above backup command, and then uncomment the volume and command listed in the `docker-compose.yml` file.

___

## Tag Versioning

- Tag verisoning is accomplished by using the [Tag-CI-CD module](https://github.com/design-group/ignition-tag-cicd-module). This can be used to avoid saving tags in the gateway backup, and instead save them in a git repository. Some helper scripts are provided to assist in the process.

## tag-import.sh

### Tag Import Description

- This script is used to import tags from the file system to a target Ignition Gateway.

#### Tag Import Usage

- Run the script with the following command:

  ```sh
  ./scripts/tag-import.sh
  ```

- The following flags are available:
  - `-p` or `--provider` The tag provider to import to. Defaults to `default`.
  - `-c` or `--policy` Set the tag import policy.
  - `-d` or `--directory` Specify the directory to import tags from.
  - `-a` or `--auto` Automatically import tags from all providers in the specified directory.
  - `--force` Force overwrite if collision policy is set to `o`.
  - `--debug` Enable debug mode.
  - `-h` or `--help` Display the help message.

## tag-export.sh

### Tag Export Description

- This script is used to export tags from a source Ignition Gateway to your file system.

## Tag Export Usage

- Run the script with the following command:

  ```sh
  ./scripts/tag-export.sh
  ```

- The following flags are available:
  - `-h` or `--help`: Display the help message.
  - `-p` or `--provider` The tag provider to export from. Defaults to `default`.
  - `-t` or `--base-tag-path` The base tag path to export from. Defaults to `/`.
  - `-r` or `--recursive` Recursively export tags. Defaults to `true`.
  - `-f` or `--file-path` The file path to export the tags to.
  - `-h` or `--help` Display the help message.
  - `-d` or `--delete-existing` Delete the existing tags in the file path.
