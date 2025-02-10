# Instructions to Install MaCh3 on CEDER Canada Computing Account
For detailed instructions on installing MaCh3, please refer to the [MaCh3 Installation Guide by Mo](https://github.com/mojiastonybrook/Documentation_Logs/blob/main/MaCh3_Installation_on_ComputeCanada_Clusters.md).

## Step 0: Login to CEDER Canada Computing Account

To login to your CEDER Canada computing account, use the following command:

```bash
ssh -XY hadhikar@cedar.computecanada.ca
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
export CMAKE_PREFIX_PATH=$CMAKE_PREFIX_PATH:$ROOTSYS
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

## Alternative Step: Copy Required Files from Beluga Cluster

If you encounter issues running the `setup.sh` file to access CMT and GSL, you can use an alternative method suggested by Mo. Copy the required files from the Beluga cluster using the following commands:

```bash
cd ..
scp hadhikar@beluga.canadacompute.ca:/home/hadhikar/mach3/DBarrow_Jointfit/CMTv1r20p20081118.tar.gz .
scp hadhikar@beluga.canadacompute.ca:/home/hadhikar/mach3/DBarrow_Jointfit/gsl-1.16.tar.gz .
```

These commands will copy the `CMTv1r20p20081118.tar.gz` and `gsl-1.16.tar.gz` files to your `Mach3_JoinFit` directory.

## Step 7: Move Back to Mach3_JoinFit Directory and Source SetMeUp.sh

To move back to the `Mach3_JoinFit` directory and source the `SetMeUp.sh` script, use the following commands:

```bash
cd ../Mach3_JoinFit
source SetMeUp.sh
```

This will set up the necessary environment variables and paths for MaCh3.

When you source the `SetMeUp.sh` script, you will see the following output in the terminal. Follow the yes/no prompts accordingly. This will take quite long time to setup.

```
Looks like you're running this in a MaCh3 folder
I'll cd /home/hadhikar/Mach3_JoinFit/MaCh3/.. and install programs there

Installing to /home/hadhikar/Mach3_JoinFit

Build ROOT? (y/n)n
Build CMT (needed for psyche)? (y/n)y
Build iRODS and get files? (y/n)n
Setup NIWGReWeight? (y/n)y
Build CMAKE binary? (y/n)n

This will build: ROOT  false
                 CMT   true
                 iRODS false
                 NIWG true
                 CMAKE false
```
## Step 8: Compile NIWGReWeight Separately

If `NIWGReWeight` is not installed successfully in Step 7, you can compile it separately by running the following command:

```bash
source setup_niwgreweight.sh
```

Then, set up the computing environments on the cluster again by running the following commands:

```bash
module load nixpkgs/16.09
module load gcc/5.4.0
module load gsl/1.16
module load root/5.34.36
module load cuda/8.0.44

export CUDAPATH=${CUDA_HOME}
```

## Step 9: Install CUDAProbs3

To install CUDAProbs3, run the following command:

```bash
source setup_CUDAProb3.sh
```

This will set up the necessary environment and install CUDAProbs3 on the cluster.

## Alternative Step: Download Psyche from Google Drive

If you encounter issues running the `setup_psyche.sh` script, you can download Psyche from the following Google Drive links:

- [Psyche Download Link 1](https://drive.google.com/drive/folders/1xkEHJ9ol4B2vimJIsDxuqCzhyk-TcwbC?usp=share_link)
- [Psyche Download Link 2](https://drive.google.com/drive/folders/1tWZkWmGnEzILmcc7wfNobkf5Qt_sgq05?usp=share_link)

After downloading, extract the files to the appropriate directory in your `Mach3_JoinFit` folder.
## Step 10: Modify Setup Scripts and Source setup_psyche.sh

After downloading and extracting the files, you need to modify the paths in the following setup scripts:

1. `setup_niwgreweight.sh`
2. `setup_CUDAProb3.sh`
3. `SetMeUp.sh`

Update the paths to point to the correct directories where the files are located.

Once the paths are updated, source the `setup_psyche.sh` script using the following command:

```bash
source setup_psyche.sh
```

This will complete the setup process for MaCh3 on the CEDER Canada computing account.

## Step 11: Download and Extract Psyche Files

1. Download the zip files from the provided Google Drive links.
2. Use `scp` to copy the downloaded files to your MaCh3 installation working directory:

    ```bash
    scp /path/to/downloaded/file.zip hadhikar@cedar.computecanada.ca:/home/hadhikar/Mach3_JoinFit/MaCh3
    ```

3. Unzip the files in the `MaCh3` directory:

    ```bash
    unzip /home/hadhikar/Mach3_JoinFit/MaCh3/file.zip -d /home/hadhikar/Mach3_JoinFit/MaCh3
    ```

## Step 12: Modify Setup Scripts

1. Open the `/home/hadhikar/Mach3_JoinFit/MaCh3/nd280Psyche/v3r47/cmt/setup.sh` file and modify the following lines:

    ```bash
    CMTROOT=/home/hadhikar/Mach3_JoinFit/CMT/v1r20p20081118
    -path=/home/hadhikar/Mach3_JoinFit/MaCh3
    ```

This command will search for all files named `setup.sh` in the current working directory and its subdirectories, and print their full paths.
To find all `setup.sh` files in all subdirectories of the working directory and change the path from `/home/mojia/MaCh3_2023/` to `/home/hadhikar/Mach3_JoinFit/` in all of those files, use the following command:

```bash
find $(pwd) -type f -name "setup.sh" -exec sed -i 's|/home/mojia/MaCh3_2023/|/home/hadhikar/Mach3_JoinFit/|g' {} +
```

To verify that all `setup.sh` file paths have been correctly changed and then source `setup_Psyche.sh`, use the following commands:

```bash
# Verify the changes in all setup.sh files
grep -r "/home/hadhikar/Mach3_JoinFit/" $(pwd) --include="setup.sh"

# Source the setup_Psyche.sh script
source /home/hadhikar/Mach3_JoinFit/MaCh3/setup_psyche.sh
```

When the setup is complete, you should see the following message indicating the successful installation of the required components:

```
nd280Psyche:          v3r47
psycheCore:         v3r35
psycheEventModel:   v3r33
psycheIO:           v3r25
psycheND280Utils:   v3r47
psychePolicy:      
psycheSelections:   v3r41
psycheSteering:     v3r33
psycheSystematics:  v3r37
psycheUtils:        v3r29
```

## Step 13: Source setup_T2KSKTools.sh

To set up T2K-SK tools, source the `setup_T2KSKTools.sh` script using the following command:

```bash
export ROOT_DIR=$(root-config --prefix)
source setup_T2KSKTools.sh
```

This will configure the necessary environment for T2K-SK tools on the cluster.

## Step 14: Download and Set Up t2ksk-common

To download the `t2ksk-common` package and check out the correct version, follow the commands in the `setup_T2KSKTools.sh` script. Use the following commands:

```bash
git clone git@github.com:t2k-software/t2ksk-common.git
cd t2ksk-common
git checkout <correct_version>
```

When you run the setup script, you might encounter the following error message:

```
-- Detecting CXX compile features - done
CMake Error at CMakeLists.txt:54 (find_package):
    By not providing "FindROOT.cmake" in CMAKE_MODULE_PATH this project has
    asked CMake to find a package configuration file provided by "ROOT", but
    CMake did not find one.

    Could not find a package configuration file provided by "ROOT" with any of
    the following names:

        ROOTConfig.cmake
        root-config.cmake

    Add the installation prefix of "ROOT" to CMAKE_PREFIX_PATH or set
    "ROOT_DIR" to a directory containing one of the above files.  If "ROOT"
    provides a separate development package or SDK, be sure it has been
    installed.

-- Configuring incomplete, errors occurred!
See also "/home/hadhikar/Mach3_JoinFit/MaCh3/T2KSKTools/t2ksk-detcovmat/CMakeFiles/CMakeOutput.log".
make: *** No targets specified and no makefile found.  Stop.
```
To resolve this, open the `CMakeLists.txt` file in `/home/hadhikar/Mach3_JoinFit/MaCh3/T2KSKTools/t2ksk-detcovmat/CMakeFiles/` and `/home/hadhikar/Mach3_JoinFit/MaCh3/T2KSKTools/t2ksk-common/CMakeFiles/` and change the following line (basically add root):

```cmake
list(APPEND CMAKE_MODULE_PATH $ENV{ROOTSYS}/etc/cmake)
```

to:

```cmake
list(APPEND CMAKE_MODULE_PATH $ENV{ROOTSYS}/etc/root/cmake)

Then in `/home/hadhikar/Mach3_JoinFit/MaCh3/T2KSKTools/t2ksk-common/CMakeFiles/` directory, run the following command:

```bash
cmake .
```

You should see output similar to the following:

```
-- Found ROOT: /cvmfs/soft.computecanada.ca/easybuild/software/2017/avx2/Compiler/gcc5.4/root/5.34.36/bin/root-config
-- Configuring done
-- Generating done
-- Build files have been written to: /home/hadhikar/Mach3_JoinFit/MaCh3/T2KSKTools/t2ksk-common
```

To compile the project using number of core:ws, run the following command:

```bash
make -j12
```

You should see output similar to the following:

```
In file included from /home/hadhikar/Mach3_JoinFit/MaCh3/Prob3++/BargerPropagator.h:5:0,
                 from /home/hadhikar/Mach3_JoinFit/MaCh3/Prob3++/BargerPropagator.cc:1:
/home/hadhikar/Mach3_JoinFit/MaCh3/Prob3++/NeutrinoPropagator.h:18:68: warning: unused parameter ‘kSetProfile’ [-Wunused-parameter]
      virtual void DefinePath( double , double , bool kSetProfile = true  )
                                                                    ^
/home/hadhikar/Mach3_JoinFit/MaCh3/Prob3++/BargerPropagator.cc:10:42: warning: unused parameter ‘k’ [-Wunused-parameter]
 BargerPropagator::BargerPropagator( bool k )
                                          ^
[ 95%] Built target t2ksk_flux_rw
[100%] Linking CXX shared library lib/libt2ksk_osc_rw.so
[100%] Built target t2ksk_osc_rw
```


Then move to `t2ksk-detcovmat` directory using the following command:

```bash
cd ../t2ksk-detcovmat/
```

Compile the project after successfully implementing `t2ksk-common` and `eigen` packages using the following command:

```bash
cmake -Dt2ksk-common_DIR=${T2KSKCOMMON} -DEigen3_DIR=${EIGENINSTALL}/share/eigen3/cmake .
```

You should see output similar to the following:

```
-- Configuring done
-- Generating done
-- Build files have been written to: /home/hadhikar/Mach3_JoinFit/MaCh3/T2KSKTools/t2ksk-detcovmat
```

To compile the project using the number of cores, run the following command:

```bash
make -j12
```

You should see output similar to the following:

```
[ 95%] Linking CXX executable bin/Testing
[ 97%] Linking CXX executable bin/validateCategories
[ 97%] Built target Testing
[ 97%] Built target validateCategories
[100%] Linking CXX executable bin/makeDetCovMatrix
[100%] Built target makeDetCovMatrix
```

## Step 15: Modify setup.sh and Compile

1. Open the `setup.sh` file and comment out the line that sources `thisroot.sh`:

    ```bash
    # source ${ROOTSYS}/bin/thisroot.sh
    ```

2. Ensure that CUDA version 8.0.44 is being used:

    ```bash
    module load cuda/8.0.44
    ```

3. Source the modified `setup.sh` script:

    ```bash
    source setup.sh
    ```

4. Clean and compile the project:

    ```bash
    make clean && make
    ```

This will ensure that the environment is correctly set up and the project is compiled with the specified CUDA version.

## Possible Error Messages

During the installation and setup process, you may encounter various error messages. Here are some common ones and their possible solutions:

Type 1:

```
g++ -shared "-Wl,-export-dynamic" -pthread -m64 -I/cvmfs/soft.computecanada.ca/easybuild/software/2017/avx2/Compiler/gcc5.4/root/5.34.36/include/root -I../  libMCMC.a mcmc.o tune.o stretch.o MCMCProcessor.o -o ../lib/libMCMC.so  -L/cvmfs/soft.computecanada.ca/easybuild/software/2017/avx2/Compiler/gcc5.4/root/5.34.36/lib/root -lGui -lCore -lCint -lRIO -lNet -lHist -lGraf -lGraf3d -lGpad -lTree -lRint -lPostscript -lMatrix -lPhysics -lMathCore -lThread -pthread -lm -ldl -rdynamic  -lMinuit -L../lib -lCovariance -lSamplePDF -lManager
make[1]: Leaving directory '/home/hadhikar/Mach3_JoinFit/MaCh3/mcmc'
make -j4 -C utils
make[1]: Entering directory '/home/hadhikar/Mach3_JoinFit/MaCh3/utils'
rootcint -f contours_biprobabilityDict.C -c Linkdef.h
rootcint -f makeND280SplinesDict.C -c Linkdef.h
g++ -O3 -g -Wall -pthread -m64 -I/cvmfs/soft.computecanada.ca/easybuild/software/2017/avx2/Compiler/gcc5.4/root/5.34.36/include/root -std=c++11 -fopenmp -I../ -I../utils/agf/include/ -I/home/hadhikar/Mach3_JoinFit/gsl-1.16/Linux/include -I/opt/ppd/t2k/users/barrowd/T2KSKTools/t2ksk-common/include -I/opt/ppd/t2k/users/barrowd/T2KSKTools/t2ksk-detcovmat/include -I../psycheinc/ -o ../bin/makeND280Splines makeND280Splines.cpp makeND280SplinesDict.C -L../lib -lThrowParms -lThreeProb_1.00 -lOscillator_1.00 -lSamplePDF -lCovariance -lMCMC -lSplines -lManager -lNIWGReWeight -lt2ksk_MR_mock_rw -lt2ksk_NC1pi_rw -lt2ksk_FV_rw -lt2ksk_numu_CCother_rw -lt2ksk_numu_CC_rw -lt2ksk_atmospheric_fit_rw -lt2ksk_NCoth_rw -lt2ksk_NCGamma_rw -lt2ksk_atmospheric_flux_rw -lt2ksk_hpi0_rw -lt2ksk_numu_NC_rw -lt2ksk_numu_PID_rw -lt2ksk_DIF_muon_rw -lt2ksk_MR_atmospheric_rw -lt2ksk_detector_covmatrix_binning -lt2ksk_numu_nue_rw -lt2ksk_decay_e_rw -Wl,--start-group -lpsycheUtils -lpsycheND280Utils -lpsycheSelections -lpsycheCore -lpsycheSystematics -lpsycheSteering -lpsycheIO -lpsycheEventModel -Wl,--end-group -L/cvmfs/soft.computecanada.ca/easybuild/software/2017/avx2/Compiler/gcc5.4/root/5.34.36/lib/root -lCore -lCint -lRIO -lNet -lHist -lGraf -lGraf3d -lGpad -lTree -lRint -lPostscript -lMatrix -lPhysics -lMathCore -lThread -pthread -lm -ldl -rdynamic -lEG -lGeom -lRooFit -lRooFitCore -lXMLIO  -L/home/hadhikar/Mach3_JoinFit/gsl-1.16/Linux/lib -lgsl -lgslcblas -lm   -fpermissive -Wl,-rpath,../lib -DMULTITHREAD
/home/hadhikar/Mach3_JoinFit/gsl-1.16/Linux/lib/libgsl.so: undefined reference to `hypot@GLIBC_2.35'
/home/hadhikar/Mach3_JoinFit/gsl-1.16/Linux/lib/libgsl.so: undefined reference to `pow@GLIBC_2.29'
/home/hadhikar/Mach3_JoinFit/gsl-1.16/Linux/lib/libgsl.so: undefined reference to `exp@GLIBC_2.29'
/home/hadhikar/Mach3_JoinFit/gsl-1.16/Linux/lib/libgsl.so: undefined reference to `log@GLIBC_2.29'
collect2: error: ld returned 1 exit status
make[1]: *** [Makefile:49: ../bin/makeND280Splines] Error 1
make[1]: Leaving directory '/home/hadhikar/Mach3_JoinFit/MaCh3/utils'
make: *** [Makefile:55: all] Error 2
```