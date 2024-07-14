# Developer setup

Follow this guide to set up your laptop with all the tools you need for the LSDS course.

> [!CAUTION]
> You are expected to complete all the steps in this guide BEFORE the first seminar or lab session takes place.

## Office hours
If you get stuck and need help, come to the office hours:
- [DD/MM/YYYY HH:MM - HH:MM CET](meet.google.com/xxx-yyyy-zzz)
- [DD/MM/YYYY HH:MM - HH:MM CET](meet.google.com/xxx-yyyy-zzz)

## Choose your group

- Fill in [this spreadsheet](todo) with your group members.

## Installation

### IDE

- [Install VSCode](https://code.visualstudio.com/download)
- [Install the Python extension](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
- [Install the Markdown Preview Mermaid Support extension](https://marketplace.visualstudio.com/items?itemName=bierner.markdown-mermaid)
- [Install the Markdown Alert extension](https://marketplace.visualstudio.com/items?itemName=yahyabatulu.vscode-markdown-alert)

### Version Control

- [Install GitHub Desktop](https://desktop.github.com/download/)

### Docker

- [Install Docker Desktop](https://www.docker.com/products/docker-desktop/)

### Operating System

- If you use Windows, [install WSL](https://canonical-ubuntu-wsl.readthedocs-hosted.com/en/latest/guides/install-ubuntu-wsl2/).

- If you use Windows, [change the default terminal in VSCode to WSL](https://stackoverflow.com/questions/44435697/change-the-default-terminal-in-visual-studio-code).

### AWS

> [!WARNING]
> If you use WSL, remember to run these commands in the WSL terminal.

- [Install the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

### Python

> [!WARNING]
> If you use WSL, remember to run these commands in the WSL terminal.

- Verify if you have Python installed

    ```zsh
    $ python3 --version
    Python 3.10.12
    ```

- If you don't, [install Python](https://www.python.org/downloads/)


## Creating your repository

- Fork this repository to create a copy with name `lsds-25-{groupnumber}`. [Help](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo#forking-a-repository)

- Mark your fork as private. [Help](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/managing-repository-settings/setting-repository-visibility#changing-a-repositorys-visibility)

- Share the repository with [all teachers](./TEACHER_LIST.md). [Help](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-access-to-your-personal-repositories/inviting-collaborators-to-a-personal-repository#inviting-a-collaborator-to-a-personal-repository)

- Share the repository with the other two members of your group. [Help](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-access-to-your-personal-repositories/inviting-collaborators-to-a-personal-repository#inviting-a-collaborator-to-a-personal-repository)

> [!WARNING]
> If you use WSL, clone your repository to the following path: `\\wsl.localhost\Ubuntu\home\{username}\lsds`.
> 
> Everything inside `\\wsl.localhost` is the file system of your Linux subsystem.

- Clone the repository using Github Desktop. [Help](https://docs.github.com/en/desktop/adding-and-cloning-repositories/cloning-a-repository-from-github-to-github-desktop)


- Fill in the [README](./README.md) with the information about your group.

- Commit and push your changes.

## Making sure Python and Docker work


> [!WARNING]
> If you use WSL, remember to run these commands in the WSL terminal.

- Run a Python server

```zsh
cd fastapi-quickstart
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
fastapi dev app/main.py
```

- Check you can access the server
```zsh
$ curl -X GET http://127.0.0.1:8000 | jq
{
  "Hello": "World"
}
```

- Stop the server

- Open Docker Desktop

- Run a Python server with Docker

```zsh
cd fastapi-quickstart
docker compose up --build
```

- Check you can access the server when it runs in Docker

```zsh
$ curl -X GET http://127.0.0.1:8001/info | jq
{
  "studentId": 123,
  "universityName": "upf"
}
```

- Try changing the code in [main.py](./resources/fastapi-quickstart/app/main.py) so it returns `555` as the `studentId`.

- Redeploy the service
```zsh
cd fastapi-quickstart
docker compose down
docker compose up --build
```

- Check it works:

```zsh
$ curl -X GET http://127.0.0.1:8001/info | jq
{
  "studentId": 555,
  "universityName": "upf"
}
```

## Catch up with everything you should know already

We expect you to understand some topics from previous courses (i.e. Networks). If you don't, here are some links you should watch before the first lab:

- **Docker.** Watch [100+ Docker Concepts you Need to Know](https://www.youtube.com/watch?v=rIrNIzy6U_g&t=143s)
- **API.** Follow the [FastAPI in Containers tutorial](https://fastapi.tiangolo.com/deployment/docker/)
- **REST API.** Watch [RESTful APIs in 100 Seconds](https://www.youtube.com/watch?v=-MTSQjw5DrM)
- **curl.** Watch [You NEED to know how to use CURL!](https://www.youtube.com/watch?v=q2sqkvXzsw8)
- **Servers.** Watch [Web Server Concepts and Examples](https://www.youtube.com/watch?v=9J1nJOivdyw)

