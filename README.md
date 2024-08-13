# NETLAB HPC

This repsitory is a guide to start using the HPC of NetLab. Please
read carefully before using the cluster. Feel free to reach out to 
the system administrator, Ali Sönmez, whenever you need assistance.

## Access

To access the HPC server, you must be either on eduroam or the CMPE 
network. For remote access, follow the documentation of the university's
[VPN service](https://bilgiislem.bogazici.edu.tr/tr/ogrenciler-icin-vpn-hizmeti).
You will be able to access the server using `ssh` after establishing a connection
to the VPN service.

```bash
ssh name-surname@79.123.176.180
```

You will be asked for a password. This is the password you are provided with.
For convenience you can import your ssh key to the server via `ssh-import-id`.
Thus, you will be able to login without a password if one of your SSH keys on
your computer matches with the one on the server.

## An Example Slurm Script

Create a new file and copy paste the content below:

```bash
#!/bin/bash
#SBATCH --container-image ghcr.io\#netlab-cluster/default
#SBATCH --gpus=1
#SBATCH --cpus-per-gpu=8
#SBATCH --mem-per-gpu=40G
#SBATCH --container-mounts /home/your-username:/mnt/your-username
#SBATCH --job-name=your-username
cd /mnt/your-username

PYTHON_FILE_NAME="file_name.py"

source /opt/python3/venv/base/bin/activate

# Run the Python file
python3 $PYTHON_FILE_NAME
```

