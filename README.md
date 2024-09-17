# JANUS Artifact Appendix

Paper title: **Janus: Fast Privacy-Preserving Data Provenance For TLS**

Artifacts HotCRP Id: **#2**

Requested Badge: **Available**, **Functional**, and **Reproduced**

## Description
The artifacts linked in this repository contribute to the creation of Figures and Tables in our research paper [Janus](https://tum-esi.github.io/publications-list/PDF/2025-PETS-Janus.pdf).
The work Janus is a protocol to verify the provenance of TLS data using advanced cryptographic techniques (secure two-party computation, paillier encryption, etc.).
We provide artifacts in the form of secure computation circuits and security protocols (ECTF conversion) which implement the optimizations proposed in our paper.

### Security/Privacy Issues and Ethical Concerns (All badges)
Janus is a research project and our implementation has *not* undergone any security audit. Do not use our Janus building blocks in a production system and do not use our building blocks to process sensitive/confidential data.
Our implementation makes use of other repositories which we do not monitor for changes (e.g. vulnerability patches or similar).
We neither audited the implementations of external repositories with respect to security problems and we solely rely on the functionalities they provide.
Be aware that using the referenced tools enable to online interaction with external websites and services.
This allows external services to learn networking and query data of your devices.
Further, make sure to obtain data processing agreements with any external site that is queried with our tools.
If you run our building blocks in the provided `local` modes, then all data exchanges happen locally and nothing is shared over the Internet.

## Basic Requirements (Only for Functional and Reproduced badges)
In order to run most of the provided artifacts, no special hardware is required.
With regard to software requirements, we explain tooling and additional requirements below.  
The expected time to run the examples are assumed to align with the benchmarks we provide in our paper.

### Hardware Requirements
This work does not require any specific hardware to reproduce the results.
We used a MacBook Pro 16-inch (2021) configured with an Apple M1 Pro chip and 32 GB of RAM.
All tests and executions have been executed on the defined laptop.

### Software Requirements
Our work entirely relies on publicly accessible software.
The following itemization highlights the software stack required to execute the building blocks we provide.
- MacOS Sonoma Version 14.6.1, [installation](https://support.apple.com/en-us/105111)
- Golang 1.21.1 darwin/arm64, [installation](https://go.dev/doc/install)
- Docker client/server version 20.10.12, [installation](https://docs.docker.com/engine/install/)
- Git version 2.39.3, [installation](https://git-scm.com/download/mac)
- Rust (cargo version 1.78.0), [installation](https://www.rust-lang.org/tools/install)

### Estimated Time and Storage Consumption
Running all repositories consequtively will roughly take about 32 minutes and the number composes as follows:
- 10 minutes for running the Janus scalability evaluation provided in Figure 9.
- 1 minute to recreate the benchmarks of DiStefano (Table 4)
- 2 minutes to recreate the benachmarks of Origo (Table 4)
- 2 minutes to recreate the benchmarks of TLSnotary (Table 4)
- 3 minutes to recreate the benchmarks of Deco/DecoProxy (Table 4)
- 1 minute to run the threeparty handshake and the consequtive ECTF conversion algorithm (Table 7)
- 3 minutes to run Janus 2PC circuits (Table 7)
- 10 minutes to read docs and set up the repositories.

The disk requirements remain below 5GB and the RAM allocation depends on the availability of resources. Our building blocks can be executed on a laptop with 8 GB of RAM.

## Environment 
In order to run the code of the Janus building blocks, only an installation of Git and Golang are required.
For running our Deco reimplementation, Docker is required.
For any benchmarks of related works, we reference the github repositories which we used for the reproduction and reference their documentation which we followed to collect benchmarks.

In the following, we describe how to access our artifacts and all related and necessary data and software components.
Afterwards, we describe how to set up everything and how to verify that everything is set up correctly.

### Accessibility (All badges)
The artifacts can be accessed from a laptop with Internet connectivity.
You are required to download some Github repositories and execute the code.

### Set up the environment (Only for Functional and Reproduced badges)
Doe to the fact that we publish a collection of building blocks, we descrive the set up requirements per repository that we share and provide a separate description of the environment.

We expect that the following commands are executed from a fixed location such as `/home/user/janus/`
To download each of the building blocks, make sure to download the following list of repositories with the commands:

- (Janus 2PC circuits) run `git clone https://github.com/jplaui/circuits_janus.git`, then `cd` into the folder `circuits_janus/2pc_circuits/tls_mpc/apps/garbled` and run `go build .`
- (ZKP circuits) run `git clone https://github.com/jplaui/gnark_lib.git`, then `cd` into the folder `circuits` and run the command `go mod tidy`. 
- (Deco reimplementation) run `git clone https://github.com/jplaui/decoTls12MtE.git`, then `cd` into the folder and run the script `./install.sh`. Next, run the command `docker build -t deco .` to build the Docker image. 
- (Origo reproduction, Janus ECTF, Janus Threeparty handshake) run the command `git clone https://github.com/jplaui/origo.git`, next `cd` into the folder `origo`.
For accessing the threeparty handshake in the proxy mode, make sure to pull the repository and switch to the branch `threephs` with the command `git clone https://github.com/jplaui/origo.git --branch threephs`.
- (DiStefano reproduction) run the command `git clone https://github.com/brave-experiments/DiStefano` and follow the installation instructions on [this page](https://github.com/brave-experiments/DiStefano/blob/main/src/README.md). Make sure to run the command `cmake ../ -DBENCHMARKING=ON`.
- (TLSnotary reproduction) run the command `git clone https://github.com/tlsnotary/tlsn.git` and follow the installation instructions [here](https://docs.tlsnotary.org/quick_start/rust.html#notarizing-private-information-discord-message)

### Testing the Environment (Only for Functional and Reproduced badges)
In order to make sure that the software has been set up correctly, please go through the following list of bullets and make sure that the expected outputs can be recreated.
- (Docker) run `docker version` and the expected output should indicate the versions 20.10.12 for the client and server-side software
- (Git) run `git version` and expect version 2.39.3
- (Golang) run `go version` and expect version go1.21.1
- (Janus 2PC circuits) run `./garbled` and the output should be `No input files`.
- (Deco reimplementation) run the command `docker images` and you should see an image called `deco`.
- (Origo reproduction, Janus ECTF, Janus threeparty handshake) run the command `go mod tidy` in each of the folders `origo/client`, `origo/proxy`, `origo/server`.
- (DiStefano reproduction) run the command `./DeriveCircuits` to create circuit which you should see as new files in the `../2pc/key-derivation` directory.
- (TLSnotary reproduction) run the commadn `cd tlsn/examples/simple` and then `cargo run --release --example simple_prover`. Next you should expect the output `Starting an MPC TLS connection with the server...`.

## Artifact Evaluation (Only for Functional and Reproduced badges)
This section includes all the steps required to evaluate our artifact's functionality and validate our paper's key results and claims.
We highlight our paper's main results and claims in the first subsection. And describe the experiments that support our claims in the subsection after that.

### Main Results and Claims
In total, we provide three main and one side claims in the context of our work. 
We mention additional other claims that can be drawn from the benchmarks we provide with respect to related works.

#### Main Result 1: Janus scalability
Describe the results in 1 to 3 sentences.
Refer to the related sections in your paper and reference the experiments that support this result/claim.

#### Main Result 2: Janus end-to-end performance

#### Main Result 3: Janus 2PC building blocks

#### Side Result 4: Current TLS cipher suite landscape


### Experiments 
Our list provided next explains each experiment which involves the following parts:
 - How to execute the experiment in detailed steps.
 - What is the expected result.
 - How long does the experiments take and how much space do they consume on disk. (approximately)
 - Which claim and results does our experiments support, and how.

#### Experiment 1: DiStefano reproduction
You need two terminal sessions and in the first session, you must run the command `./E2EBench --is_server --ip 127.0.0.1 ` and use the output you obtain in oder to launch the command in the second terminal session. The command should be similar to the following command `./E2EBench --ip 127.0.0.1 -p 54349 -v 54348`.
The output will be a list of benchmarks which you have to add together (respecting offline, online bandwidth and execution times) in order to obtain the benchmarks we showcase in Table 4. The results supports our claims of our main result 2.

#### Experiment 2: Deco reimplementation
Run the command `docker run -it deco` to start the Docker container. 
Now you need 2 additional terminal sessions. 
From each of your in total 3 terminal sessions, run the command `docker exec -it CONTAINERID /bin/bash`, where you can get the `CONTAINERID` value in the first column if you run the command `docker ps -a | grep deco`.
After you connected to the container using three terminal sessions, make sure to use the first terminal session and `cd` to the folder `app/server`, use the second terminal session and `cd` to the folder `app/verifier` and use the third terminal session and `cd` to the folder `app/prover`.
Once the prover starts the execution of the Deco TLS oracle, you will see the command line outputs that indicate the benchmarks that we used to create the Deco and DecoProxy entries in our Table 4. The results supports our claims of our main result 2.

#### Experiment 4: Janus Circuits

The results supports our claims of our main result 2.

#### Experiment 5: Janus ZKP (zkSNARK) Circuit Benchmarks


#### Experiment 6: TLS cipher suite analysis

The results supports our claims made in our minor result 4.

#### Experiment 7: Janus ECTF, Origo benchmarks

The results supports our claims made in our main result 2 and contribute to the main result 3.

#### Experiment 7: Janus three party handshake
The results supports our claims made in our main result 2.


## Limitations (Only for Functional and Reproduced badges)
We do not provide a full end-to-end implementation of Janus yet due to the fact that our ongoing work of integrating Janus into the browser might require additional unknown changes/removals of the our main code structure.

## Notes on Reusability (Only for Functional and Reproduced badges)
Our building blocks or sub parts can be reused to compose different types of secure channel protocols. For instance, we extract our ECTF algorithm and at the same time show how it is used in the crypto/tls Golang standard package.
Equally, all semi-honest and malicious 2PC benchmarks as well as our ZKP circuits can be reused as gadgets.
We specifically highlight that our implementations of AES128 and SHA256 have been the first publicly diclosed implementations in gnark.
The research community already improved the versions we provided which benefited our work.
Further, we link a repository which implements a proxy deployment of TLS oracles including a SNI-based routing of session establishments.
