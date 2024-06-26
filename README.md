# lakefs-trial
Short example on using LakeFS for use with MAST data.

We have decided to not pursue this approach for data versioning on the FAIR MAST project, and this repo exsits purely as a guide for any future work that may want to use LakeFS as a service. Justification for not using LakeFS is outlined in `lakefs_downsides.docx`. 
## Development Setup

### Mac Users:

If you are using Mac for development, use [podman](https://podman.io/docs/installation) instead of docker. Follow the installation guide to set it up, then follow the below set up. 

### Linux/Windows Users:

If using Linux or Windows, you need to make sure you have [docker](https://www.docker.com/get-started/) and `docker-compose` installed on your system.

### Setup

We will be using the Python package manager [uv](https://astral.sh/blog/uv) to install our dependencies. As a first step, make sure this is installed with:
```bash
pip install uv
```
Secondly, clone the repository:
```bash
git clone git@github.com:jameshod5/lakefs-trial.git
cd lakefs-trial
```

You can use either `conda` or `venv` to set up the environment. Follow the below instructions for venv:

### Using venv
Ensure you are using Python version `3.11`:
```bash
uv venv venv
source venv/bin/activate
uv pip install -r requirements.txt
```

Use `uv --help` for additional commands, or refer to the documentation if needed.

### Start the Data Management System
Run the development container to start the lakefs repo, minio storage and the bucket. You need to populate your own bucket, for this test case place the `30420.zarr` file inside the bucket. Follow the below command using s5cmd as this will ensure the hidden metadata files are in your bucket. Make sure the bucket is named `zarr-example` in order for the notebooks to work well.

```bash
s5cmd --endpoint-url http://localhost:9000 cp 30420.zarr s3://zarr-example/
```

### Mac Users:

```bash
podman compose \
--env-file dev/docker/.env.dev  \
-f dev/docker/docker-compose.yml \
up \
--build
```

Podman does not shutdown containers on its own, unlike Docker. To shutdown Podman completely run:

```bash
podman compose -f dev/docker/docker-compose.yml down   
podman volume rm --all
```

## Set up LakeFS and LakeCTL

When you run the containers, you will notice LakeFS gives you a set up link. Follow this link and fill the email with any email (it doesn't matter). 

You will be guided to a page containing secret ID and access key. KEEP THIS PAGE OPEN (or download them). Follow the log in link and copy and paste the details in. You are now inside LakeFS.

Finally, you will need LakeCTL (assuming you are Mac):

```bash
brew tap treeverse/lakefs
brew install lakefs`
```

Set up your config with:

```bash
lakectl config
```

And enter the same details as you did to login to LakeFS. Host is http://127.0.0.1:8000

## lakefs-notebook.ipynb
 
This notebook goes through using lakefs-spec for file navigation, commits and branch management. 

## zarr-lakefs.ipynb

This notebook goes through using the Python SDK for LakeFS, opening and changing a Zarr file.