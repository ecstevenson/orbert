# Docker compose

## Basic commands

Build and run:
```
docker-compose up -d --build
```

Run (need to run in docker-compose directory, with correct path to Dockerfile, under "context", else will try to rebuild):
```
docker-compose up -d
```

Get logs and http:
```
docker-compose logs
```

Or if your docker-compose file has multiple "services":
```
docker-compose logs {{service_name}}
docker-compose logs jupyter
```

Open a terminal in the container:
```
docker-compose exec jupyter bash
```

Shut down the container:
```
docker-compose down
```

Checks:
```
docker ps -a
docker stats
nvidia-smi
nvtop
```

### Docker & detach

Using Screen and docker exec you can open a bash terminal and disconnect your ssh se
ssion.

```
screen -S <session name>
docker-compose up -d
docker-compose exec jupyter bash
python3 <>.py
```
Then detach with ```Ctrl-a``` + ```Ctrl-d```.

To resume:
```
screen -ls
screen -r
```

## Set up

Create a ```.env``` file in the docker directory or the root of the project (meant to be deployed in a system with at least one GPU):

```
# The name of the docker-compose project (i.e. wp8-long-term-space-environment) (controls the image and container names)
COMPOSE_PROJECT_NAME=your_project_name
# The user ID you are using to run docker-compose (from >> echo $UID, or >> id)
USER_ID=your_numeric_id
# The group ID you are using to run docker-compose (from >> id -g)
GROUP_ID=your_numeric_id
# The user name assigned to the user id
USER_NAME=your_user_name
# The port from which you want to access Jupyter lab (e.g. 8888, 8866)
JUPYTER_PORT=XXXX
# The path to your data files to train/test the models (e.g. ./data)
LOCAL_DATA_PATH=/path/to/your/data
# The W&B personal API key (see https://wandb.ai/authorize)
WANDB_API_KEY=your_wandb_api_key
# List of comma separated GPU indices that will be available in the container (by default only 0, the first one)
CUDA_VISIBLE_DEVICES=0
# Github PAT (see https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and>
GH_TOKEN=your_github_pat
```

Install the sshfs driver for accessing files on a remote machine:
```
docker plugin install --grant-all-permissions vieux/sshfs
```

Ensure all file permissions are the same ```ls -l```
