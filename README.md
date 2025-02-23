# rapidsnark

rapid snark is a zkSnark proof generation written in C++ and intel assembly. That generates proofs created in [circom](https://github.com/iden3/snarkjs) and [snarkjs](https://github.com/iden3/circom) very fast.

## Dependencies

You should have installed gcc, libsodium, and gmp (development)

In ubuntu:

````
sudo apt install build-essential
sudo apt install cmake
sudo apt-get install libgmp-dev
sudo apt-get install libsodium-dev
sudo apt-get install nasm
````

## Compile prover in standalone mode

````sh
npm install
git submodule init
git submodule update
npx task createFieldSources
npx task buildProver
````

## Compile prover in server mode

````sh
npm install
git submodule init
git submodule update
npx task createFieldSources
npx task buildPistache
npx task buildProverServer
````

## Building proof

You have a full prover compiled in the build directory.

So you can replace snarkjs command:

````sh
snarkjs groth16 prove <circuit.zkey> <witness.wtns> <proof.json> <public.json>
````

by this one
````sh
./build/prover <circuit.zkey> <witness.wtns> <proof.json> <public.json>
````
## Launch prover in server mode
````sh
./build/proverServer  <port> <circuit1_zkey> <circuit2_zkey> ... <circuitN_zkey>
````
For every `circuit.circom` you have to generate with circom with --c option the `circuit_cpp` and after compilation you have to copy the executable into the `build` folder so the server can generate the witness and then the proof based on this witness.
You have an example of the usage calling the server endpoints to generate the proof with Nodejs in `/tools/request.js`.

To test a request you should pass an `input.json` as a parameter to the request call.
````sh
node tools/request.js <input.json> <circuit>
````
## Benchmark

This prover uses intel assembly with ADX extensions and parallelizes as much as it can the proof generation.

The prover is much faster that snarkjs and faster than bellman.

[TODO] Some comparation tests should be done.


## License

rapidsnark is part of the iden3 project copyright 2021 0KIMS association and published with GPL-3 license. Please check the COPYING file for more details.
