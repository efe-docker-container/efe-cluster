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
ssh <name-surname>@79.123.176.180
```

You will be asked for a password. This is the password you are provided with.
For convenience you can import your ssh key to the server via `ssh-import-id`.
Thus, you will be able to login without a password if one of your SSH keys on
your computer matches with the one on the server.

_NOTE: Before using `ssh-import-id`, please read [doc1](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
and [doc2](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
carefully._

## Example Slurm Scripts

Create a new file and copy paste the content below:

```bash
#!/bin/bash
#SBATCH --container-image ghcr.io\#netlab-cluster/default
#SBATCH --gpus=1
#SBATCH --cpus-per-gpu=4
#SBATCH --mem-per-gpu=40G
#SBATCH --container-mounts /home/<your-username>:/mnt/<your-username>
#SBATCH --job-name=<your-username>

cd /mnt/<your-username>

PYTHON_FILE_NAME="<file_name.py>"

# Run the Python file
python3 $PYTHON_FILE_NAME
```

Here is a brief explanation of the Slurm directives:

- `#SBATCH --container-image ghcr.io\#netlab-cluster/default`: Specifies the 
container image to be used for the job. This image includes CUDA support, pytorch,
numpy, and pandas.

- `#SBATCH --gpus=1`: Requests one GPU for the job. **Note**: If your job does not require a GPU,
you are advised not to include this command and, thus, not to allocate any GPUs to your Slurm 
job unnecessarily.

- `#SBATCH --cpus-per-gpu=8`: Specifies that each GPU should access 8 CPU cores.

- `#SBATCH --mem-per-gpu=40G`: Allocates 40 GB of RAM per GPU.

- `#SBATCH --container-mounts /home/<your-username>:/mnt/<your-username>`: Mounts your home 
directory to the container, allowing the container to access files from your home directory.
**Note**: Thus, keep your job submission script, like the one above, in your home directory
and provide the PYTHON_FILE_NAME variable relative to the home directory. For example, if your
Python file called sim.py is in a directory called simulation, then your PYTHON_FILE_NAME 
variable should be simulation/sim.py.

Below is an example script whose job does not use any gpu, but uses cpus:

```bash
#!/bin/bash
#SBATCH --container-image ghcr.io\#netlab-cluster/default
#SBATCH --cpus-per-task=4
#SBATCH --mem=40G
#SBATCH --container-mounts /home/<your-username>:/mnt/<your-username>
#SBATCH --job-name=<your-username>

cd /mnt/<your-username>

PYTHON_FILE_NAME="<file_name.py>"

# Run the Python file
python3 $PYTHON_FILE_NAME
```

## Submit Your Job

Navigate to the directory where you saved you job submission script and submit the job using the
following `sbatch` command:

```bash
sbatch <job-submission-script.sh>
```

When you submit a job to the Slurm scheduler, it will process the request and execute the job 
within the specified containerized environment. The output of the job will be written to a file
named slurm-<job-id>.out, where you can review the results and any printed output.

## Admin's Final Notes

I strongly encourage you to read the Slurm documentation for `sinfo`, `squeue`, `srun`, `sbatch`, 
`scancel`, as well as a documentation for `scp` for writing the code in your environment and faster file 
transfers between your computers and the server. Familiarizing yourself with these commands will
deepen your understanding of Slurm. Feel free to contact me related to errors you encounter or 
with any suggestions for system improvements.

