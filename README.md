# Stone Packaging

This project provides various forms of distribution for [Stone](https://github.com/starkware-libs/stone-prover) executables.

## About

The goal of this project is to reduce friction and speed up the process of generating proofs using Stone. More broadly, the aim is to make Stone a &#34;known&#34; piece of infrastructure that can be easily integrated into application-specific workflows and maintained efficiently.

## Roadmap

- [x] Static binary releases for x86_64
- [x] Static binary releases for ARM64
- [x] Minimal Docker images for x86_64
- [x] Native packages for Debian/Ubuntu

Follow-up work:
- Native packages for Fedora
- Native packages for Alpine
- Homebrew packages
- Technical documentation for file formats (inputs, outputs, memory, trace, proof), and test data
- Documentation hosted on GitHub Pages
- Integrated proof decomposition (related to https://github.com/zksecurity/stark-evm-adapter)
- Observability suite (metrics, dashboards, configurable logging)
- Stwo support

## Usage Instructions

### Download Binaries for x86_64

```bash
sudo wget https://github.com/dipdup-io/stone-packaging/releases/latest/download/cpu_air_prover-x86_64 -O /usr/local/bin/cpu_air_prover && sudo chmod +x /usr/local/bin/cpu_air_prover

sudo wget https://github.com/dipdup-io/stone-packaging/releases/latest/download/cpu_air_verifier-x86_64 -O /usr/local/bin/cpu_air_verifier && sudo chmod +x /usr/local/bin/cpu_air_verifier
```

### Download Binaries for macOS ARM64

```bash
wget https://github.com/dipdup-io/stone-packaging/releases/latest/download/cpu_air_prover-arm64 -O /usr/local/bin/cpu_air_prover && chmod +x /usr/local/bin/cpu_air_prover

wget https://github.com/dipdup-io/stone-packaging/releases/latest/download/cpu_air_verifier-arm64 -O /usr/local/bin/cpu_air_verifier && chmod +x /usr/local/bin/cpu_air_verifier
```

### Creating and Verifying a Test Proof Using Binaries

Clone the repository:

```bash
git clone https://github.com/dipdup-io/stone-packaging.git /tmp/stone-packaging
```

Navigate to the example test directory:

```bash
cd /tmp/stone-packaging/test_files/
```

Copy or download the binary files from the latest release to this directory.

Run the prover:
```bash
cpu_air_prover \
    --out_file=fibonacci_proof.json \
    --private_input_file=fibonacci_private_input.json \
    --public_input_file=fibonacci_public_input.json \
    --prover_config_file=cpu_air_prover_config.json \
    --parameter_file=cpu_air_params.json
```

The proof will be available at `fibonacci_proof.json`.

Run the verifier to verify the proof:

```bash
cpu_air_verifier --in_file=fibonacci_proof.json && echo "Successfully verified example proof."
```

## Download Minimal Docker Images for x86_64

Download the Docker image. The stone-prover package includes both cpu_air_prover and cpu_air_verifier:

```bash
docker pull ghcr.io/dipdup-io/stone-packaging/stone-prover:latest
```

### Creating and Verifying a Test Proof Using Docker

Clone the repository:

```bash
git clone https://github.com/dipdup-io/stone-packaging.git /tmp/stone-packaging
```

Run the Docker container with a volume mounted:

```bash
docker run --entrypoint /bin/bash -v /tmp/stone-packaging/test_files:/app/prover ghcr.io/dipdup-io/stone-packaging/stone-prover -c "cd /app/prover && exec cpu_air_prover \
    --out_file=fibonacci_proof.json \
    --private_input_file=fibonacci_private_input.json \
    --public_input_file=fibonacci_public_input.json \
    --prover_config_file=cpu_air_prover_config.json \
    --parameter_file=cpu_air_params.json"
```

The proof will be available at test_files/fibonacci_proof.json.

To verify the proof:

```bash
docker run --entrypoint /bin/bash -v /tmp/stone-packaging/test_files:/app/prover ghcr.io/dipdup-io/stone-packaging/stone-prover -c "cd /app/prover && exec cpu_air_verifier --in_file=fibonacci_proof.json && echo 'Successfully verified example proof.'"
```

## Download Native Packages for Debian/Ubuntu

Download the .deb package from the latest release:

```bash
wget https://github.com/dipdup-io/stone-packaging/releases/latest/download/stone-prover-linux-x86_64.deb && sudo dpkg -i stone-prover-linux-x86_64.deb
```

### Creating and Verifying a Test Proof Using the .deb Package

Clone the repository:

```bash
git clone https://github.com/dipdup-io/stone-packaging.git /tmp/stone-packaging
```

Navigate to the example test directory:

```bash
cd /tmp/stone-packaging/test_files/
```

Run the prover:
```bash
cpu_air_prover \
    --out_file=fibonacci_proof.json \
    --private_input_file=fibonacci_private_input.json \
    --public_input_file=fibonacci_public_input.json \
    --prover_config_file=cpu_air_prover_config.json \
    --parameter_file=cpu_air_params.json
```

The proof will be at `fibonacci_proof.json`.

Run the verifier to confirm the proof:
```bash
cpu_air_verifier --in_file=fibonacci_proof.json && echo "Successfully verified example proof."
```

## Docker Image Publishing

This repository contains a GitHub Actions workflow that automatically builds and publishes Docker images to Docker Hub.

- **Workflow file:** `.github/workflows/docker-publish.yml`

### Usage

1. Set up the following GitHub repository secrets under **Settings** > **Secrets and variables** > **Actions**:
   - `DOCKER_LOGIN`: Your Docker Hub username.
   - `DOCKERHUB_TOKEN`: Your Docker Hub access token.
2. The workflow is triggered by any push or pull request to the `master` branch. It builds the Docker image and publishes it to Docker Hub.

### Workflow Overview

The workflow performs the following actions:
- Checks out the repository code.
- Sets up Docker Buildx for cross-platform builds.
- Logs in to Docker Hub using the credentials stored in GitHub Secrets.
- Builds the Docker image based on the repository content.
- Pushes the image to Docker Hub with the specified tag.
- Generates an attestation for the image artifact (not applicable to forked repositories).
- Logs out of Docker Hub after the process is complete.

### Tests

1. Forked the original repository.
2. To use the local Docker Hub, update the workflow file by setting `env.DOCKER_HUB` to `127.0.0.1:5000`.
3. Temporarily updated the workflow trigger branch for testing purposes.
4. Pushed a small change to trigger the workflow.
5. Monitored the workflow in the **Actions** tab.
6. Verified the Docker image was pushed to local Docker Hub.

This project is supported by Nethermind and Starknet Foundation via [OnlyDust platform](https://app.onlydust.com/p/stone-packaging-)


### USNG VOCS TO GENERATE DOCUMENTATION LOCALHOST SITE

We use Vocs to generate our documentation site. Here's how to set it up and run it locally:

### Prerequisites

- Node.js (version 14 or later)
- Yarn package manager

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/dipdup-io/stone-packaging.git
   cd stone-packaging
   ```

2. Install dependencies:
   ```bash
   yarn install
   ```

### Running the Development Server

To start the development server:

```bash
yarn dev
```

This will start the server at `http://localhost:5173`. The site will automatically reload if you make changes to the source files.

### Building the Static Site

To build the static site:

```bash
yarn build
```

This will generate the static files in the `dist` directory.

### Project Structure

- `docs/`: Contains all the documentation markdown files.
- `docs/pages/`: Contains the main content pages.
- `docs/index.md`: The home page of the documentation.
- `vocs.config.ts`: Configuration file for Vocs.

### Adding New Pages

To add a new page to the documentation:

1. Create a new markdown file in the `docs/pages/` directory.
2. Add the new page to the sidebar in `vocs.config.ts`.
