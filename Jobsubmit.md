# MaCh3 Validation study by Haradhan Adhikary
This document serves as a comprehensive guide to the MaCh3 validation study conducted after installing MaCh3 on the CEDAR ComputeCanada cluster. It details all the steps followed to perform the validation test, including potential sources of errors encountered during the study and suggestions from experts.

## Prerequisites
Before starting the validation process, ensure that the following prerequisites are met:
- Access to the ComputeCanada cluster (CEDAR)
- MaCh3 framework installed on the cluster
- Basic knowledge of SLURM job scheduling

## Steps Overview
1. **Setup Environment**: Configure the environment and load necessary modules.
2. **Data Preparation**: Prepare the input data required for the validation.
3. **Job Submission**: Submit jobs to the cluster using SLURM scripts.
4. **Monitoring Jobs**: Monitor the job status and handle any errors.
5. **Post-Processing**: Analyze the output data and validate the results.

## Potential Errors and Solutions
- **Module Load Failures**: Ensure all required modules are available and correctly loaded.
- **Job Submission Errors**: Verify SLURM script syntax and resource allocation.
- **Data Access Issues**: Confirm that input data paths are correct and accessible.

## Expert Suggestions
- Regularly consult with experts to address any issues and improve the validation process.
- Document all steps and errors encountered for future reference.

By following this guide, I will be able to validate your MaCh3 installation and perform the necessary tests to ensure its proper functionality on the CEDAR ComputeCanada cluster.

## Validations of T2K-SK Joint Fit with MaCh3 on ComputeCanada Cluster

This document outlines the steps and validations performed for the T2K-SK joint fit using the MaCh3 framework on the ComputeCanada cluster.

## Table of Contents
1. [Introduction](#introduction)
2. [Setup](#setup)
3. [Running the Fit](#running-the-fit)
4. [Validation](#validation)
5. [Results](#results)
6. [Conclusion](#conclusion)

### Introduction
Provide a brief overview of the T2K-SK joint fit and the purpose of using the MaCh3 framework on the ComputeCanada cluster. This documentation describes the pre-fitting validations performed by the version of MaCh3 being used in the T2K-SK joint analysis in 2023. The examples are processed on the ComputeCanada cluster Cedar, including running the executables of PrintEventRate, AtmSigmaVar and SystLLHScan.

Note: All the job scripts to be submitted to a remote node should be put in the \project space of ComputeCanada. The \home space of ComputeCanada does not allow slurm to access.

### Print Event Rate
The event rates of the samples covered by the joint fit are recorded in the tech-note [TN471's table 2 and table 3](https://t2k.org/docs/technotes/471/JointFitFitterValidv1.2).

#### Knowlegede from Technical Note:
The first joint oscillation analysis of SK atmospheric neutrinos and T2K accelerator neutrinos is performed by three fitters. P-theta and MaCh3 are adopted from the T2K analysis and Osc3++ is adopted from the SK analysis. This note describes the cross-fitter validation processes including the comparison of MC event rates, the event rate and likelihood responses as a function of each systematic parameter, and sensitivity study using the Asimov data set.

### Asimov Data

The Asimov data set is a hypothetical data set used in statistical analyses to represent the expected outcome of an experiment assuming a particular model is true. It is named after the science fiction writer Isaac Asimov. In the context of the T2K-SK joint fit, the Asimov data set is used to perform sensitivity studies and validate the fitting process.

#### Generating Asimov Data
To generate the Asimov data set for the T2K-SK joint fit, follow these steps:

1. **Define the True Parameters**: Specify the true values of the parameters for which you want to generate the Asimov data. These parameters could include oscillation parameters, systematic uncertainties, and other relevant quantities.

2. **Simulate the Expected Event Rates**: Using the MaCh3 framework, simulate the expected event rates for the defined true parameters. This involves running the `PrintEventRate` executable with the appropriate configuration files.

3. **Create the Asimov Data Set**: The simulated event rates are then used to create the Asimov data set. This data set represents the expected number of events in each bin of the analysis, assuming the true parameters are correct.

#### Example
Here is an example of how to generate the Asimov data set using the MaCh3 framework:

```bash
# Load the necessary modules
module load mach3

# Define the configuration file for the true parameters
cat > true_params.cfg <<EOL
oscillation_parameters:
    theta23: 0.5
    delta_cp: 1.57
    ...
systematic_uncertainties:
    flux_norm: 1.0
    detector_eff: 1.0
    ...
EOL

# Run the PrintEventRate executable to simulate the expected event rates
PrintEventRate --config true_params.cfg --output asimov_event_rates.root

# The output file 'asimov_event_rates.root' contains the Asimov data set
```

#### Using Asimov Data for Sensitivity Studies
Once the Asimov data set is generated, it can be used to perform sensitivity studies. These studies involve fitting the Asimov data set with different hypotheses and comparing the results to evaluate the sensitivity of the experiment to various parameters.

For example, to perform a sensitivity study on the oscillation parameter `theta23`, you can fit the Asimov data set with different values of `theta23` and calculate the likelihood for each fit. The resulting likelihood curve can then be used to determine the sensitivity of the experiment to `theta23`.

```bash
# Fit the Asimov data set with different values of theta23
for theta23 in $(seq 0.4 0.01 0.6); do
    FitAsimovData --data asimov_event_rates.root --param theta23 --value $theta23 --output fit_results_$theta23.root
done

# Analyze the fit results to determine the sensitivity
AnalyzeFitResults --input fit_results_*.root --output sensitivity_curve.png
```

By following these steps, you can generate and use the Asimov data set to validate the T2K-SK joint fit and perform sensitivity studies using the MaCh3 framework on the ComputeCanada cluster.



