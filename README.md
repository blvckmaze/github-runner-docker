# github-runner-docker

This project provides a Dockerized solution for running Github Actions runners. Github Actions are a powerful tool for automating software workflows. Despite the **[statement][runner-documentation]** made by the actions runner maintainers that you cannot use Docker to create runners, this project shows that you actually can. It allows you to easily spin up a Docker container that can be used as a runner for your Github Actions, enabling more efficient testing and deployment of the workflows.

This project uses an Ubuntu-based Docker-in-Docker (dind) image created by **[cruizba][image-author]**, which can be found **[here][image-repo]**. This choice was made due to problems that occurred during the installation of requirements on the official Alpine-based dind image.

In this example, dind is used because my workflows consist of running dockerized tests. However, your workflows might be different, so feel free to experiment with other images.

## How to install

## Requirements

- Install **[docker engine][docker-link]** (20.10.0+) / **[docker compose][docker-compose-link]**.
- To use Docker containers without switching to superuser mode, you will need to add a group for the current system user by running: `sudo usermod -aG docker $USER` <br/>
  Otherwise, you will have to run all Docker commands through the sudo command.

### Setting up local environment

- Copy **[.env.example][env-example]** contents and create new `.env` file with it.
- Update environment variables in newly created `.env` file, get runner token by visiting `"Settings > Actions > Runners > New self-hosted runner"` in your repository and copying it from `Configure` block
- Build and start container by running the following command inside project's root folder: `docker-compose up -d` 
- In the future if you'll need to update token/repository you'll need to rebuild container by running: `docker-compose up -d --build` 

## Configuration

### Environment variables

| Variable name          | Description                                                                                                                                             | Default value                                     |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| ACTIONS_RUNNER_TOKEN   | This is an authentication token used by the Github Actions runner to communicate with the Github API and access resources in your repository.           | AQJIKZAAABBBCCCDDDEEECHANGEME                     |
| ACTIONS_RUNNER_VERSION | This variable specifies the version of the Github Actions runner that will be used.                                                                     | 2.303.0                                           |
| REPO_URL               | This is the URL of the Github repository where the actions runner will be used. The runner will be able to access and run workflows in this repository. | https://github.com/blvckmaze/github-runner-docker |

[docker-link]: https://docs.docker.com/engine/install/
[docker-compose-link]: https://docs.docker.com/compose/install/
[env-example]: .env.example
[runner-documentation]: https://github.com/actions/runner/blob/main/docs/automate.md#why-cant-i-use-a-container
[image-author]: https://github.com/cruizba
[image-repo]: https://github.com/cruizba/ubuntu-dind
