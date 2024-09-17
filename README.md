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

- (Janus 2PC circuits and zkSNARK circuits) run `git clone https://github.com/jplaui/circuits_janus.git`, then `cd` into the folder `circuits_janus/2pc_circuits/tls_mpc/apps/garbled` and run `go build .`
In order to run the zkSNARK circuits, `cd` into the folder `circuits_janus/gnark_zkp` and run `go mod init gnark_zkp` as well as `go mod tidy`.
- (Deco reimplementation) run `git clone https://github.com/jplaui/decoTls12MtE.git`, then `cd` into the folder and run the script `./install.sh`. Next, run the command `docker build -t deco .` to build the Docker image. 
- (Origo reproduction, Janus ECTF, Janus Threeparty handshake) run the command `git clone https://github.com/jplaui/origo.git`, next `cd` into the folder `origo` and run the local installation git submodule commands provided [here](https://github.com/jplaui/origo/blob/main/docs/01_installation.md#locally-running-the-repo).
- (DiStefano reproduction) run the command `git clone https://github.com/brave-experiments/DiStefano` and follow the installation instructions on [this page](https://github.com/brave-experiments/DiStefano/blob/main/src/README.md). Make sure to run the command `cmake ../ -DBENCHMARKING=ON`.
- (TLSnotary reproduction) run the command `git clone https://github.com/tlsnotary/tlsn.git` and follow the installation instructions [here](https://docs.tlsnotary.org/quick_start/rust.html#notarizing-private-information-discord-message)
- (TLS scanner) run `git clone https://github.com/TeoLj/TLSscanner.git` and follow the [installation instructions](https://github.com/TeoLj/TLSscanner?tab=readme-ov-file#installation).

### Testing the Environment (Only for Functional and Reproduced badges)
In order to make sure that the software has been set up correctly, please go through the following list of bullets and make sure that the expected outputs can be recreated.
- (Docker) run `docker version` and the expected output should indicate the versions 20.10.12 for the client and server-side software
- (Git) run `git version` and expect version 2.39.3
- (Golang) run `go version` and expect version go1.21.1
- (Janus 2PC circuits) run `./garbled` and the output should be `No input files`. Further, in the `gnark_zkp` folder, make sure that running `go run main.go -debug -tls13-zkopen -byte-size 256 -iterations 1` yields the message `DBG verifier done backend=groth16 curve=bn254` somewhere in the output.
- (Deco reimplementation) run the command `docker images` and you should see an image called `deco`.
- (Origo reproduction, Janus ECTF, Janus threeparty handshake) run the command `go mod tidy` in each of the folders `origo/client`, `origo/proxy`, `origo/server`.
- (DiStefano reproduction) run the command `./DeriveCircuits` to create circuit which you should see as new files in the `../2pc/key-derivation` directory.
- (TLSnotary reproduction) run the commadn `cd tlsn/examples/simple` and then `cargo run --release --example simple_prover`. Next you should expect the output `Starting an MPC TLS connection with the server...`.
- (TLS scanner) make sure to run the command `go run .` and expect the multiple options such as `-domains` the output.

## Artifact Evaluation (Only for Functional and Reproduced badges)
This section includes all the steps required to evaluate our artifact's functionality and validate our paper's key results and claims.
We highlight our paper's main results and claims in the first subsection. And describe the experiments that support our claims in the subsection after that.

### Main Results and Claims
In total, we provide three main and one side claims in the context of our work. 
We mention additional other claims that can be drawn from the benchmarks we provide with respect to related works.

#### Main Result 1: Janus scalability
Our results of Figure 9 show how the hvzk circuits of Janus (semi-honest 2PC circuits) outperform related TLS data opening circuits which are executed using zkSNARK systems.
The experiments 4 and 5 support this claim.

#### Main Result 2: Janus end-to-end performance
Our results of Table 4 indicate that Janus can prove private TLS data of kilobyte sizes in seconds (post record computation). Further, we show how Janus behaves (practical in handshake phase and partly improved record phase, most efficient post-record phase) compared to related open-source tools for data provenance verification.
The experiments 1, 2, 3, 4, 5, and 7 contribute to this claim.

#### Main Result 3: Janus 2PC building blocks
We show that Janus secure computation building blocks are practical as stated via Table 7.
The experiments 4 and 7 support this claim.

#### Side Result 4: Current TLS cipher suite landscape
We show that today's TLS 1.3 support is very close to today's TLS 1.2 support in Figure 13.
The claim has been made based on the insights of the experiment 6.

### Experiments 
Our list provided next explains each experiment which involves the following parts:
 - How to execute the experiment in detailed steps.
 - What is the expected result.
 - How long does the experiments take and how much space do they consume on disk. (approximately)
 - Which claim and results does our experiments support, and how.

#### Experiment 1: DiStefano reproduction
You need two terminal sessions and in the first session, you must run the command `./E2EBench --is_server --ip 127.0.0.1 ` and use the output you obtain in oder to launch the command in the second terminal session. The command should be similar to the following command `./E2EBench --ip 127.0.0.1 -p 54349 -v 54348`.
The output will be a list of benchmarks which you have to add together (respecting offline, online bandwidth and execution times) in order to obtain the benchmarks we showcase in Table 4. The results supports our claims of our main result 2.
This experiment does not take long and has no drastic consumption of system resources.

#### Experiment 2: Deco reimplementation
Run the command `docker run -it deco` to start the Docker container. 
Now you need 2 additional terminal sessions. 
From each of your in total 3 terminal sessions, run the command `docker exec -it CONTAINERID /bin/bash`, where you can get the `CONTAINERID` value in the first column if you run the command `docker ps -a | grep deco`.
After you connected to the container using three terminal sessions, make sure to use the first terminal session and `cd` to the folder `app/server`, use the second terminal session and `cd` to the folder `app/verifier` and use the third terminal session and `cd` to the folder `app/prover`.
Once the prover starts the execution of the Deco TLS oracle, you will see the command line outputs that indicate the benchmarks that we used to create the Deco and DecoProxy entries in our Table 4.
This experiment does not take long and has no drastic consumption of system resources except for the sizes of the container images.
The results supports our claims of our main result 2.

#### Experiment 3: TLSnotary reproduction
Please follow the documentation provided [here](https://docs.tlsnotary.org/quick_start/rust.html#notarizing-private-information-discord-message) to execute TLS notary. We measured this execution with external tools (Wireshark to collect bandwidth benchmarks) and we added rust timing measurements to the code to measure execution times.
This experiment does not take long and has no drastic consumption of system resources.
The results supports our claims made in our main result 2.

#### Experiment 4: Janus Circuits
In order to run the semi-honest 2PC circuits of Janus, two terminal sessions are required. Make sure you are in the location `circuits_janus/2pc_circuits/tls_mpc/apps/garbled`. In the first window, make sure the run the evaluator first with a command similar to `MPCLDIR=./../../ ./garbled -e -v -i 0x0101010101010101010101010101010101010101010101010101010101010101 examples/janusV2_12MtE32B.mpcl`. Afterwards, in the second terminal session (same location as noted above), run the command `MPCLDIR=./../../ ./garbled -v -i 0x0ebf8610f78fa0455f887e03166d715620dd847b687ad16e97405751f5e5bf77 examples/janusV2_12MtE32B.mpcl`.
In every file we provide (every file inside the folder `circuits_janus/2pc_circuits/tls_mpc/apps/garbled/examples` that starts with `janus*`), you can find a sample command that executes the 2PC evaluator and garbler. We have put all commands above the struct definitions of the garbler and the evaluator (e.g. check [this file](https://github.com/jplaui/circuits_janus/blob/main/2pc_circuits/tls_mpc/apps/garbled/examples/janusV2_12MtE32B.mpcl)).
You can find all our results in the [evaluation file](https://github.com/jplaui/circuits_janus/blob/main/2pc_circuits/tls_mpc/apps/garbled/examples/janusV2_evaluation.md), where we accumulate all our benchmarks. Make sure to render our evaluation file using Markdown.
We use the circuits found in the files with the filenames MtE, private, and transparent with the different sizes for the creation of hvzk benchmarks in Figure 9.
Further, we have used to following python code snippet (we used ipynb noteboot in the tool colab.research.google.com):
```bash
## python version
!python --version
## test python print
!sudo apt install cm-super dvipng texlive-latex-extra texlive-latex-recommended
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

%matplotlib --list
# required for using matplotlib in colab
%matplotlib inline

tex_fonts = {
    # Use LaTeX to write all text
    "text.usetex": True,
    "font.family": "serif",
    # Use 10pt font in plots, to match 10pt font in document
    "axes.labelsize": 13,
    "font.size": 13,
    # Make the legend/label fonts a little smaller
    "legend.fontsize": 12,
    "xtick.labelsize": 13,
    "ytick.labelsize": 13
}
width = 345

plt.rcParams.update(tex_fonts)

# prove times (s)
x = [16, 32, 64, 128, 256, 512, 1024, 2048, 4096, 8192]

# janusV2
#decoZk_groth16_prove_times = [0.42, 0.714, 1.37, 2.47, 5.01, 9.89, 29.57, None, None,None] # 2048 1m10.5s
decoZk_plonk_prove_times = [3.88, 7.02, 13.78, 31.23, 81.32, None, None, None, None, None] # 2048 1m10.5s
decoZk_plonkFRI_prove_times = [107, 218.05, None, None, None, None, None, None, None, None] # 2048 1m10.5s
pageSignerZk_groth16_prove_times = [0.14, 0.33, 0.64, 1.24, None, None, None, None, None, None] # 32 kB 3.83s, 64kB 7.46s, 100kb 14.34s :::::: 1.24*16=19,84 s for 2048 bits
pageSignerZk_plonk_prove_times = [0.3, 0.62, 1.04, 2.11, None, None, None, None, None, None] # 65536 have 3m9.5s
pageSignerZk_plonkFRI_prove_times = [6.61, 13.34, 26.98, 54.73, None, None, None, None, None, None] # 4096 1m59.66s works as well
# sha256_groth16_prove_times = [0.06, 0.072, 0.129, 0.143, 0.274, 0.492, 1.07, 1.97, 4.32, 8.20] #idx-11=16.49
# sha256 hmac and last 2 aes decryptions
sha256_groth16_prove_times = [0.76, 0.772, 0.829, 0.843, 0.974, 1.692, 1.77, 2.67, 5.02, 8.90]
# aes plus authtag 2 aes decryptions
decoZk_groth16_prove_times = [1.12, 1.414, 2.07, 3.17, 5.71, 10.59, 30.37, None, None,None]

# evalaute janus
janusV2_proveZkOpen_times = [0.28, 0.28, 0.31, 0.36, 0.46, 0.7, 1.01, 1.74, 3.29, 6.7]
janusV2_proveTranspOpen_times = [0.25, 0.26, 0.26, 0.27, 0.27, 0.28, 0.29, 0.31, 0.39, 0.58]

janusV2_prove12Mte_times = [0.08, 0.088, 0.116, 0.17, 0.267, 0.452, 0.831, 1.59, 3.09, 6.2]

garbleThenProve_proveTranspOpen_times = [None, None, None, None, 0.46 ,0.51, 0.61, 0.72, None, None]

fig, ax = plt.subplots(figsize=(7.5, 3))

# plot lines
plt.plot(x, decoZk_groth16_prove_times, label = "$\mathcal{C}^{zk}_{AEAD}$", linestyle="solid", color="black") # linewidth=2.5
plt.plot(x, janusV2_proveZkOpen_times, label = "$\mathcal{C}^{hvzk}_{zkOpen}$", linestyle="solid", color="red")
plt.plot(x, sha256_groth16_prove_times, label="$\mathcal{C}^{zk}_{MtE}$",linestyle="dotted", color="black")
plt.plot(x, janusV2_prove12Mte_times, label="$\mathcal{C}^{hvzk}_{MtE}$",linestyle="dotted", color="red")
plt.plot(x, janusV2_proveTranspOpen_times, label = "$\mathcal{C}^{hvzk}_{tpOpen}$", linestyle="dashdot", color="darkred")

plt.xscale('log', base=2)
plt.yscale('log', base=2)
ax.legend() # loc=''
ax.set_ylim(0, 1200)

plt.ylabel('Prove Times (s)')
plt.xlabel('Size of Data Chunks (B)')
plt.savefig("opening_times.pdf", format="pdf", bbox_inches="tight")
plt.show()
```
This experiment do not take long individually but depending on the number of experiments, the experiment consumes multiple minutes.
No drastic consumption of system resources happens.
The results supports our claims made in our main result 1 and contribute to the main result 3 which concerns Table 7.

#### Experiment 5: Janus ZKP (zkSNARK) Circuit Benchmarks
Make sure to jump into the folder `circuits_janus/gnark_zkp/circuits` and run `go mod tidy`. 
We provide the contents of our go.mod file next to show which version of gnark we use:
```bash go
module circuits

go 1.19

require (
	github.com/consensys/gnark v0.7.2-0.20230518132517-274c883477ec
	github.com/consensys/gnark-crypto v0.11.1-0.20230508024855-0cd4994b7f0b
	github.com/montanaflynn/stats v0.7.1
	github.com/rs/zerolog v1.29.1
)

require (
	github.com/bits-and-blooms/bitset v1.5.0 // indirect
	github.com/blang/semver/v4 v4.0.0 // indirect
	github.com/consensys/bavard v0.1.13 // indirect
	github.com/davecgh/go-spew v1.1.1 // indirect
	github.com/fxamacker/cbor/v2 v2.4.0 // indirect
	github.com/google/pprof v0.0.0-20230309165930-d61513b1440d // indirect
	github.com/kr/text v0.2.0 // indirect
	github.com/mattn/go-colorable v0.1.12 // indirect
	github.com/mattn/go-isatty v0.0.14 // indirect
	github.com/mmcloughlin/addchain v0.4.0 // indirect
	github.com/pmezard/go-difflib v1.0.0 // indirect
	github.com/rogpeppe/go-internal v1.10.0 // indirect
	github.com/stretchr/testify v1.8.2 // indirect
	github.com/x448/float16 v0.8.4 // indirect
	golang.org/x/crypto v0.6.0 // indirect
	golang.org/x/sys v0.5.0 // indirect
	gopkg.in/yaml.v3 v3.0.1 // indirect
	rsc.io/tmplfunc v0.0.3 // indirect
)
```
Next, you can see possible circuits with the command `go run main.go -debug -sha`. Next, use a command as `go run main.go -debug -gcm -byte-size 256 -iterations 1` to evaluate the AES_GCM zkSNARK circuit and continue to change the size of bytes for the proof using the -byte-size flag. Further, you can increase the number of executions with the -iterations flag. We use a circuits aes_gcm and sha256 to create the Figure 9 zk MtE and zk AEAD numbers (black and dotted black lines). 
You must change the algorithm flag (e.g. `go run main.go -debug -sha256 -byte-size 256 -iterations 1`) in order to change the circuit.
We use the circuits of [sha256](https://github.com/jplaui/circuits_janus/blob/main/gnark_zkp/circuits/main.go#L83) for the HMAC MtE opening algorithm in Figure 9.
This experiment takes longer the larger the byte sizes are. Further, as much system resources as possible are allocated for this job.
The results supports our claims made in our main result 1.

#### Experiment 6: TLS cipher suite analysis
In order to run the cipher suite analysis, make sure to run the command `go run . -csv=top-1m.csv -entries=15000` and the tool yields the results in form of an .html file. We took the graph and included it in our work (Figure 13).
This experiment takes a few minutes (e.g. 5 depending on your Internet connectivity) but has no drastic consumption of system resources.
The results supports our claims made in our minor result 4.

#### Experiment 7: Janus ECTF, Origo benchmarks
In order to run the ECTF algorithm and get the Origo reproduction, you must launch three terminal sessions. Jump with each of the terminal session into the folders `origo/server`, `origo/proxy`, `origo/client` and now start in the server location with the command `go run main.go -debug`. Next run the proxy (location inside `origo/proxy`) with the command `go run main.go -listen` and run the client (location inside `origo/client`) with the command `go run main.go -debug -request`.
After the client command, the code executes the threeparty handshake and the ECTF algorithm and outputs the bandwidth and execution time benchmarks (see [code](https://github.com/jplaui/tls_fork/blob/88b41e47f1e91b83b63ab04e4b9d9df11fbc92df/new_ectf.go#L313)). 
In order to get the Origo end to end benchmarks, continue the execution of the command sequence provided [here](https://github.com/jplaui/origo/tree/main?tab=readme-ov-file#running-the-protocol-locally). You already did step 3. at this point.
This experiment does not take long and has no drastic consumption of system resources.
The results supports our claims made in our main result 2 and contribute to the main result 3.

Notice that this experiment also executes the threeparty handshake, where the [makeClientHello](https://github.com/jplaui/tls_fork/blob/client/handshake_client.go#L151) function adds a new keyshare which is supposed to be generated at the proxy.
Further, we generate the secret shared client DHE [here](https://github.com/jplaui/tls_fork/blob/client/handshake_client_tls13.go#L395).
The 3PHS is executed locally and the code snipped provided here does not run the randomness addition to the tls_fork branch of the proxy.


## Limitations (Only for Functional and Reproduced badges)
We do not provide a full end-to-end implementation of Janus yet due to the fact that our ongoing work of integrating Janus into the browser might require additional unknown changes/removals of the our main code structure.
However, as promised in the paper, we open-source all secure computation building blocks.

## Notes on Reusability (Only for Functional and Reproduced badges)
Our building blocks or sub parts can be reused to compose different types of secure channel protocols. For instance, we extract our ECTF algorithm and at the same time show how it is used in the crypto/tls Golang standard package.
Equally, all semi-honest and malicious 2PC benchmarks as well as our ZKP circuits can be reused as gadgets.
We specifically highlight that our implementations of AES128 and SHA256 have been the first publicly diclosed implementations in gnark.
The research community already improved the versions we provided which benefited our work.
Further, we link a repository which implements a proxy deployment of TLS oracles including a SNI-based routing of session establishments.
