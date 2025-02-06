# Instructions to Install MaCh3 on CEDER Canada Computing Account
For detailed instructions on installing MaCh3, please refer to the [MaCh3 Installation Guide by Mo](https://github.com/mojiastonybrook/Documentation_Logs/blob/main/MaCh3_Installation_on_ComputeCanada_Clusters.md).

## Step 0: Login to CEDER Canada Computing Account

To login to your CEDER Canada computing account, use the following command:

```bash
ssh -XY hadhikar@ceder.canadacompute.ca
```

After running the command, you will be prompted to enter your password. Once you have entered your password, you will need to provide the two-factor authentication code from your Duo Mobile application.

## Step 1: Create a Directory

To create a directory named `Mach3_JoinFit` in the working directory, use the following command:

```bash
mkdir -p Mach3_JoinFit
```

## Step 2: Change to the New Directory

To change your working directory to `Mach3_JoinFit`, use the following command:

```bash
cd Mach3_JoinFit
```

## Step 3: Install MaCh3

To install MaCh3, you need to clone the repository from GitHub. Before that let's create a setup.sh file:

```bash
touch setup.sh
```
Add the following content to the `setup.sh` file:

```bash
#!/bin/bash

module load nixpkgs/16.09
module load gcc/5.4.0
module load gsl/1.16
module load root/5.34.36
module load cuda/8.0.44

export CUDAPATH=${CUDA_HOME}
```

To change the mode of the `setup.sh` script and execute it to set up the environment on the cluster, use the following commands:

```bash
chmod +x setup.sh
./setup.sh
```

When the bash script is executed, the following will print on the terminal:

```
Lmod is automatically replacing "gentoo/2023" with "nixpkgs/16.09".

------------------------------------------------------------------------------------------
There are messages associated with the following module(s):
------------------------------------------------------------------------------------------

nixpkgs/16.09:
    These standard environment modules are deprecated and now contain software packages
    that have security issues. We recommend not using these, and instead use our more
    recent StdEnv modules. Use at your own risk.

------------------------------------------------------------------------------------------
Inactive Modules:
  1) flexiblas       3) libfabric/1.18.0     5) ucc/1.2.0
  2) hwloc/2.9.1     4) pmix/4.2.4           6) ucx/1.14.1

The following have been reloaded with a version change:
  1) gcc/12.3 => gcc/9.1.0                4) mii/1.1.2 => mii/1.1.1
  2) gcccore/.12.3 => gcccore/.9.1.0      5) openmpi/4.1.5 => openmpi/4.0.1
  3) imkl/2023.2.0 => imkl/2019.3.199

The following have been reloaded with a version change:
  1) gcc/9.1.0 => gcc/5.4.0               3) openmpi/4.0.1 => openmpi/2.1.1
  2) gcccore/.9.1.0 => gcccore/.5.4.0

The following have been reloaded with a version change:
  1) gsl/1.16 => gsl/2.2.1

Due to MODULEPATH changes, the following have been reloaded:
  1) openmpi/2.1.1
```
After setting up the ssh key, use the command below to download MaCh3 from its GitHub repository:

```bash
git clone git@github.com:t2k-software/MaCh3.git
```

You need to set up a personal access token on GitHub.
 * Follow the instructions here: [Creating a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)
 
When the cloning process is complete, you should see output similar to the following:

```
Cloning into 'MaCh3'...
remote: Enumerating objects: 63554, done.
remote: Counting objects: 100% (1515/1515), done.
remote: Compressing objects: 100% (665/665), done.
remote: Total 63554 (delta 1120), reused 893 (delta 835), pack-reused 62039
Receiving objects: 100% (63554/63554), 1.22 GiB | 22.34 MiB/s, done.
Resolving deltas: 100% (46117/46117), done.
Updating files: 100% (309/309), done.
```


## Step 4: Change to the MaCh3 Directory

To change your working directory to the newly cloned `MaCh3` directory, use the following command:

```bash
cd MaCh3
```

## Step 5: Check Out the DBarrow_JointFit Branch

To check out the `DBarrow_JointFit` branch, use the following command:

```bash
git checkout DBarrow_JointFit
```

When the branch is successfully checked out, you should see output similar to the following:

```
Updating files: 100% (1882/1882), done.
branch 'DBarrow_JointFit' set up to track 'origin/DBarrow_JointFit'.
Switched to a new branch 'DBarrow_JointFit'
```
## Step 6: Create an SSH Key for GitLab

To create an SSH key for GitLab, follow these steps:

1. Generate a public/private ed25519 key pair using the following command:

    ```bash
    ssh-keygen -t ed25519 -C "h.adhikary@uw.edu.pl" -f ~/.ssh/id_ed25519_gitlab
    ```

2. Check the public key using the following command:

    ```bash
    cat /home/hadhikar/.ssh/id_ed25519_gitlab.pub
    ```

3. Add this new key to your GitLab SSH keys.

4. Get an agent PID using the following command:

    ```bash
    eval "$(ssh-agent -s)"
    ```

5. Add the identity using the following command:

    ```bash
    ssh-add ~/.ssh/id_ed25519_gitlab
    ```

6. Check access to T2K GitLab using the following command:

    ```bash
    ssh -T git@git.t2k.org
    ```