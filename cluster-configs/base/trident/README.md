# Trident Operator

This directory contains the Kustomize configuration to deploy the NetApp Trident operator on OpenShift.

## Installation

To install the Trident operator, you need to:

1.  **Configure the Backend:** The `instance/trident-backend-config.yaml` file contains placeholder values (`patch-me-in-overlay`). You must create an overlay and patch the following fields with your specific backend details:
    *   `managementLIF`
    *   `dataLIF`
    *   `svm`
    *   `aggregate`
    *   `credentials.name`


