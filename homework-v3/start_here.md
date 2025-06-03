# Polkadot SDK Coding Assigment

## Overview

The goal of this assignment is to familiarize yourself with the environment Polkadot-SDK developers work in. In the repository you received, you will find a `code` directory containing the Polkadot-SDK's code.

Additionally, you will find `network.toml` and `network.zndsl` files. The [`Zombienet`](https://github.com/paritytech/zombienet) tool uses both files to easily spawn short-lived networks for testing and experimentation.

During this interview, you will perform the following tasks:

1. Build a Polkadot node and a parachain node.
2. Run a small test network and integration test.
3. Observe that the test fails and analyze the logs.
4. Search for the issue in the `code` folder.
5. Recompile and run the test again.
6. Repeat steps 3 to 5 until the test succeeds.
7. Submit your solution.

## Goal

The goal of this assignment is to find the root cause of the test failure and run the integration test specified in `network.zndsl` successfully. Currently, the test will fail because a bug has been introduced into the codebase. To fix it, you will need to run the test, analyze the log output, identify the bug in the `code` folder, fix it, and run the test again.

## Building Polkadot-SDK

To build, you need `protobuf`, `build-essential`, and a working Rust environment on your machine. 

**NOTE:** This homework assignment has been tested with Rust 1.79.0. Attempting to build with newer versions might result in errors.

You can find further setup guidance [here (please use Rust v1.79.0 still)](https://docs.polkadot.com/develop/parachains/install-polkadot-sdk/).

From inside the `code` directory, run the following commands to compile the Polkadot and parachain nodes. In addition to building, this will install the
node binaries on your machine. Rebuild the parachain node after every change you want to test:

```sh
# Builds a Polkadot node
cargo install --path ./polkadot --force --locked
# Builds a parachain node
cargo install --path ./templates/parachain/node --force --locked
```

**Note:** Polkadot-SDK is a large code base. Compiling the nodes can take a while.

## Launching the Test

To perform end-to-end tests, we employ a tool called Zombienet. Zombienet allows for the spawning of ephemeral networks and then performing assertions on them. Included with this assignment, you will find two test-related files:

- **network.toml:** A Zombienet network description file that describes a network with two relay chain nodes and a single parachain node. You can modify this file to pass additional command line flags for experimentation and debugging.
- **network.zndsl:** A Zombienet test description file. In this case, the test checks that the node _charlie_ produces at least 10 blocks within 5 minutes.

To run this test, download `v1.3.105` from the [Zombienet GitHub](https://github.com/paritytech/zombienet/releases/tag/v1.3.105) and place it in your `$PATH`. Then, run the test with the following command:

```sh
<your_zombienet_executable> -l text -d ./test-output --provider native test network.zndsl
```

The integration test will perform the following steps:

- Launch two relaychain nodes
- Launch a parachain node
- Assert that the parachain node will build 10 blocks in 5 minutes

You will see that without modification, an assertion will fail and node "charlie" is failing to build blocks.
The test will output test files and logs in the `test-output` folder. You can specify a different location for the test files by changing the `-d ./test-output` argument. Inside the folder, you will find `charlie.log`, which is the log file of the parachain node. Search this log file for indicators of the failing assertions root cause.

## Submission
Once you have finished the coding challenge, please submit the following:
1. A brief explanation of the steps you took and the reasoning you applied to find the issue.
2. The code changes you made to fix the issue. Preferable format is a patch file containing the changes.

## Notes & Tips

- You should spend around 90 minutes on this assignment.
- Blockchain knowledge is not assumed. If you are unfamiliar with the terminology, focus on the task at hand and analyze the logs to find clues about the issue.
