## Homomorphic Encryption using MSFT Seal

Needs g++ >=6 and cmake is >= 3.10, which are met by Ubuntu 18.04 but not in 16.04.


Optional for small instances: 

    free -m #to check how much memory we have
    sudo dd if=/dev/zero of=/swapfile bs=1M count=512
    #sudo dd if=/dev/zero of=/swapfile bs=1G count=2 

    sudo mkswap /swapfile
    sudo swapon /swapfile
    sudo chown root:root /swapfile
    sudo chmod 0600 /swapfile



### On Ubuntu 18.04
Very simple to install required g++ and cmake

    sudo apt-get install build-essential
    sudo apt install g++
    sudo apt install cmake

Check the verions

    g++ --version
    cmake --version


### On Ubuntu 16.04

Need G++ >= 6 (the c++ compiler from GNU compiler collection GCC)

    sudo apt-get install build-essential
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt install g++-8 # -y # for auto yes


    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 60 \
                            --slave /usr/bin/g++ g++ /usr/bin/g++-8 
    #sudo update-alternatives --config gcc #no config needed, only one version installed
    gcc --version
    g++ --version

Cmake: 
Two options: 1) binary + symlink (2) build from source 


### Using binaries for cmake
    
    wget https://github.com/Kitware/CMake/releases/download/v3.12.4/cmake-3.12.4-Linux-x86_64.sh

    sudo bash cmake-3.12.4-Linux-x86_64.sh --prefix=/opt/

    sudo ln -s /opt/cmake-3.12.4-Linux-x86_64/bin/cmake /usr/local/bin/cmake

Test the version

    cmake --version

### Using source for cmake (slow)
This will take several minutes to bootstrap and make. Change version and build numbers, or use general idea

Should already have build essentials: (prior to latest g++) sudo apt-get install build-essential

    wget https://cmake.org/files/cmake-VersionBuild.tar.gz
    tar -xzvf cmake-VersionBuild.tar.gz
    cd cmake-VersionBuild/


Install the source

    ./bootstrap
    sudo make #looks like sudo is required
    sudo make install

    cmake --version


### Installing SEAL
Once g++ --version >= 6 and cmake --version >= 3.10. Also need make obviously (if not already installed).

    sudo apt-get install make
    sudo apt-get update

Get the SEAL source code 
    
    cd
    git clone https://github.com/Microsoft/SEAL.git

Move into source directory 

    cd ~/SEAL/native/src

    cmake .
    make
    sudo make install
    cd ../..

then build the exampless

    cd ~/SEAL/native/examples
    cmake .
    make

Examples are in native/bin

    cd ~/SEAL/native/bin
    ./sealexamples

This starts the interface

    Microsoft SEAL version: 3.2.0

    SEAL Examples:

    1. BFV Basics I
    2. BFV Basics II
    3. BFV Basics III
    4. BFV Basics IV
    5. BFV Performance Test
    6. CKKS Basics I
    7. CKKS Basics II
    8. CKKS Basics III
    9. CKKS Performance Test
    0. Exit

    Total memory allocated from the current memory pool: 0 MB

    Run example: 1

Enter the number of the example to run 1-9 or 0 to quit

### Example 1:

    *******************************************
    ********** Example: BFV Basics I **********
    *******************************************

    / Encryption parameters:
    | scheme: BFV
    | poly_modulus_degree: 2048
    | coeff_modulus size: 54 bits
    | plain_modulus: 256
    \ noise_standard_deviation: 3.2

    Encoded 5 as polynomial 1x^2 + 1 (plain1)
    Encoded -7 as polynomial FFx^2 + FFx^1 + FF (plain2)
    Encrypting plain1: Done (encrypted1)
    Encrypting plain2: Done (encrypted2)
    Noise budget in encrypted1: 35 bits
    Noise budget in encrypted2: 35 bits
    Noise budget in -encrypted1: 35 bits
    Noise budget in -encrypted1 + encrypted2: 35 bits
    Noise budget in (-encrypted1 + encrypted2) * encrypted2: 16 bits
    Decrypting result: Done
    Plaintext polynomial: 2x^4 + 3x^3 + 5x^2 + 3x^1 + 2
    Decoded integer: 84


### Aside: 
The source code for the examples is in https://github.com/Microsoft/SEAL/blob/master/native/examples/examples.cpp

This has detailed (though top level) explanations for what is going on: 

raw input -> encode (poly) -> encrypt -> compute -> decrypt -> decode  

### Example 2:

    ********************************************
    ********** Example: BFV Basics II **********
    ********************************************

    / Encryption parameters:
    | scheme: BFV
    | poly_modulus_degree: 8192
    | coeff_modulus size: 218 bits
    | plain_modulus: 1024
    \ noise_standard_deviation: 3.2

    Encrypting 1x^2 + 2x^1 + 3: Done
    Size of a fresh encryption: 2
    Noise budget in fresh encryption: 196 bits
    Size after squaring: 3
    Noise budget after squaring: 174 bits
    Size after second squaring: 5
    Noise budget after second squaring: 142 bits
    Fourth power: 1x^8 + 8x^7 + 24x^6 + 68x^5 + D6x^4 + 138x^3 + 144x^2 + D8x^1 + 51

    Encrypting 1x^2 + 2x^1 + 3: Done
    Size of a fresh encryption: 2
    Noise budget in fresh encryption: 196 bits
    Size after squaring: 3
    Noise budget after squaring: 174 bits
    Size after relinearization: 2
    Noise budget after relinearizing (dbc = 16): 174 bits
    Size after second squaring: 3
    Noise budget after second squaring: 147 bits
    Size after relinearization: 2
    Noise budget after relinearizing (dbc = 16): 147 bits
    Fourth power: 1x^8 + 8x^7 + 24x^6 + 68x^5 + D6x^4 + 138x^3 + 144x^2 + D8x^1 + 51

    Encrypting 1x^2 + 2x^1 + 3: Done
    Size of a fresh encryption: 2
    Noise budget in fresh encryption: 196 bits
    Size after squaring: 3
    Noise budget after squaring: 173 bits
    Size after relinearization: 2
    Noise budget after relinearizing (dbc = 60): 142 bits
    Size after second squaring: 3
    Noise budget after second squaring: 116 bits
    Size after relinearization: 2
    Noise budget after relinearizing (dbc = 60): 116 bits
    Fourth power: 1x^8 + 8x^7 + 24x^6 + 68x^5 + D6x^4 + 138x^3 + 144x^2 + D8x^1 + 51

    Size after third squaring: 3
    Noise budget after third squaring: 87 bits
    Size after relinearization: 2
    Noise budget after relinearizing (dbc = 60): 87 bits
    Eighth power: 1x^16 + 10x^15 + 88x^14 + 310x^13 + 13Cx^12 + 110x^11 + 78x^10 + 390x^9 + 1A6x^8 + 2B0x^7 + 38x^6 + B0x^5 + 3FCx^4 + 30x^3 + 348x^2 + B0x^1 + 1A1

### Example 3:

    *********************************************
    ********** Example: BFV Basics III **********
    *********************************************

    / Encryption parameters:
    | scheme: BFV
    | poly_modulus_degree: 4096
    | coeff_modulus size: 109 bits
    | plain_modulus: 40961
    \ noise_standard_deviation: 3.2

    Batching enabled: true
    Plaintext matrix row size: 2048
    Input plaintext matrix:

        [  0,  1,  2,  3,  0, ...,  0,  0,  0,  0,  0 ]
        [  4,  5,  6,  7,  0, ...,  0,  0,  0,  0,  0 ]

    Encrypting: Done
    Noise budget in fresh encryption: 82 bits
    Second input plaintext matrix:

        [  1,  2,  1,  2,  1, ...,  2,  1,  2,  1,  2 ]
        [  1,  2,  1,  2,  1, ...,  2,  1,  2,  1,  2 ]

    Adding and squaring: Done
    Noise budget in result: 53 bits
    Decrypting result: Done
    Result plaintext matrix:

        [  1,  9,  9, 25,  1, ...,  4,  1,  4,  1,  4 ]
        [ 25, 49, 49, 81,  1, ...,  4,  1,  4,  1,  4 ]

    Unrotated matrix: 

        [  0,  1,  2,  3,  0, ...,  0,  0,  0,  0,  0 ]
        [  4,  5,  6,  7,  0, ...,  0,  0,  0,  0,  0 ]

    Noise budget in fresh encryption: 82 bits
    Rotated rows 3 steps left: 

        [  3,  0,  0,  0,  0, ...,  0,  0,  0,  1,  2 ]
        [  7,  0,  0,  0,  0, ...,  0,  0,  4,  5,  6 ]

    Noise budget after rotation: 53 bits
    Rotated columns: 

        [  7,  0,  0,  0,  0, ...,  0,  0,  4,  5,  6 ]
        [  3,  0,  0,  0,  0, ...,  0,  0,  0,  1,  2 ]

    Noise budget after rotation: 53 bits
    Rotated rows 4 steps right: 

        [  0,  4,  5,  6,  7, ...,  0,  0,  0,  0,  0 ]
        [  0,  0,  1,  2,  3, ...,  0,  0,  0,  0,  0 ]

    Noise budget after rotation: 52 bits

    Total memory allocated from the current memory pool: 74 MB
    Run example: 4

### Example 4:

    ********************************************
    ********** Example: BFV Basics IV **********
    ********************************************

    Current parms_id: 20842e2bb834b8fa c88aaa3b9c523d6d f5044207b55bb84e 57d4a76442041c42
    Changing plain_modulus ...
    Current parms_id: 25f63d1cef5230c1 721ed6848efd79f6 b4f8f8fda1bbcd67 47a346ba606d0694

    / Encryption parameters:
    | scheme: BFV
    | poly_modulus_degree: 8192
    | coeff_modulus size: 218 bits
    | plain_modulus: 1048577
    \ noise_standard_deviation: 3.2

    parms_id of public_key: 25f63d1cef5230c1 721ed6848efd79f6 b4f8f8fda1bbcd67 47a346ba606d0694
    parms_id of secret_key: 25f63d1cef5230c1 721ed6848efd79f6 b4f8f8fda1bbcd67 47a346ba606d0694
    parms_id of plain: 0 0 0 0 (not set)
    parms_id of encrypted: 25f63d1cef5230c1 721ed6848efd79f6 b4f8f8fda1bbcd67 47a346ba606d0694

    Chain index: 3
    parms_id: 25f63d1cef5230c1 721ed6848efd79f6 b4f8f8fda1bbcd67 47a346ba606d0694
    coeff_modulus primes: 7fffffff380001 7ffffffef00001 3fffffff000001 3ffffffef40001 
    \
    \-->
    Chain index: 2
    parms_id: bf69fdfc256c4eaa 430019c297419558 80211ea789d91cc5 e0ce88178c5bc078
    coeff_modulus primes: 7fffffff380001 7ffffffef00001 3fffffff000001 
    \
    \-->
    Chain index: 1
    parms_id: 89af5330ea715994 3a2c221dd7db92d4 657e068bfa1d699b fd5dfb3b08d7de93
    coeff_modulus primes: 7fffffff380001 7ffffffef00001 
    \
    \-->
    Chain index: 0
    parms_id: e16355d2310d61c1 f64af4059e5b51ee 2e98034d497fd268 44610b4cedcb7ab7
    coeff_modulus primes: 7fffffff380001 
    \
    \-->
    End of chain reached

    Chain index: 3
    parms_id of encrypted: 25f63d1cef5230c1 721ed6848efd79f6 b4f8f8fda1bbcd67 47a346ba606d0694
    Noise budget at this level: 186 bits
    \
    \-->
    Chain index: 2
    parms_id of encrypted: bf69fdfc256c4eaa 430019c297419558 80211ea789d91cc5 e0ce88178c5bc078
    Noise budget at this level: 135 bits
    \
    \-->
    Chain index: 1
    parms_id of encrypted: 89af5330ea715994 3a2c221dd7db92d4 657e068bfa1d699b fd5dfb3b08d7de93
    Noise budget at this level: 81 bits
    \
    \-->
    Chain index: 0
    parms_id of encrypted: e16355d2310d61c1 f64af4059e5b51ee 2e98034d497fd268 44610b4cedcb7ab7
    Noise budget at this level: 26 bits
    \
    \-->
    End of chain reached

    Decryption: 1x^3 + 2x^2 + 3x^1 + 4

    Noise budget before squaring: 186 bits
    Noise budget after squaring: 132 bits
    Noise budget after modulus switching: 132 bits
    Noise budget after squaring: 78 bits
    Noise budget after modulus switching: 78 bits
    Noise budget after squaring: 24 bits
    Noise budget after modulus switching: 24 bits

    Decryption of eighth power: 1x^24 + 10x^23 + 88x^22 + 330x^21 + EFCx^20 + 3A30x^19 + C0B8x^18 + 22BB0x^17 + 58666x^16 + C88D0x^15 + 98377x^14 + ECC0Ex^13 + D8B38x^12 + 42E89x^11 + D4BFFx^10 + 304x^9 + 279D4x^8 + DE654x^7 + 21FB5x^6 + 679F8x^5 + 6E5FBx^4 + E9FFEx^3 + 3BFFFx^2 + 60000x^1 + 10000

    Chain index: 0
    parms_id: 25f63d1cef5230c1 721ed6848efd79f6 b4f8f8fda1bbcd67 47a346ba606d0694
    coeff_modulus primes: 7fffffff380001 7ffffffef00001 3fffffff000001 3ffffffef40001 
    \
    \-->
    End of chain reached

    Total memory allocated from the current memory pool: 81 MB

    Run example: 5 

### Example 5

    ***************************************************
    ********** Example: BFV Performance Test **********
    ***************************************************

    / Encryption parameters:
    | scheme: BFV
    | poly_modulus_degree: 4096
    | coeff_modulus size: 109 bits
    | plain_modulus: 786433
    \ noise_standard_deviation: 3.2

    Generating secret/public keys: Done
    Generating relinearization keys (dbc = 60): Done [8538 microseconds]
    Generating Galois keys (dbc = 60): Done [190254 microseconds]
    Running tests .......... Done

    Average batch: 503 microseconds
    Average unbatch: 447 microseconds
    Average encrypt: 6865 microseconds
    Average decrypt: 4299 microseconds
    Average add: 106 microseconds
    Average multiply: 51615 microseconds
    Average multiply plain: 4606 microseconds
    Average square: 37080 microseconds
    Average relinearize: 4779 microseconds
    Average rotate rows one step: 4912 microseconds
    Average rotate rows random: 19120 microseconds
    Average rotate columns: 4901 microseconds

    / Encryption parameters:
    | scheme: BFV
    | poly_modulus_degree: 8192
    | coeff_modulus size: 218 bits
    | plain_modulus: 786433
    \ noise_standard_deviation: 3.2

    Generating secret/public keys: Done
    Generating relinearization keys (dbc = 60): Done [57961 microseconds]
    Generating Galois keys (dbc = 60): Done [1414140 microseconds]
    Running tests .......... Done

    Average batch: 1039 microseconds
    Average unbatch: 914 microseconds
    Average encrypt: 22240 microseconds
    Average decrypt: 15355 microseconds
    Average add: 390 microseconds
    Average multiply: 190086 microseconds
    Average multiply plain: 19204 microseconds
    Average square: 140362 microseconds
    Average relinearize: 27289 microseconds
    Average rotate rows one step: 27981 microseconds
    Average rotate rows random: 123641 microseconds
    Average rotate columns: 28048 microseconds

    / Encryption parameters:
    | scheme: BFV
    | poly_modulus_degree: 16384
    | coeff_modulus size: 438 bits
    | plain_modulus: 786433
    \ noise_standard_deviation: 3.2

    Generating secret/public keys: Done
    Generating relinearization keys (dbc = 60): Done [430064 microseconds]
    Generating Galois keys (dbc = 60): Done [11235591 microseconds]
    Running tests .......... Done

    Average batch: 2171 microseconds
    Average unbatch: 1917 microseconds
    Average encrypt: 79550 microseconds
    Average decrypt: 58445 microseconds
    Average add: 1512 microseconds
    Average multiply: 803802 microseconds
    Average multiply plain: 80328 microseconds
    Average square: 599178 microseconds
    Average relinearize: 179092 microseconds
    Average rotate rows one step: 180784 microseconds
    Average rotate rows random: 1039035 microseconds
    Average rotate columns: 181433 microseconds

    Total memory allocated from the current memory pool: 711 MB

    Run example: 6

### Example 6
    ********************************************
    ********** Example: CKKS Basics I **********
    ********************************************

    / Encryption parameters:
    | scheme: CKKS
    | poly_modulus_degree: 8192
    | coeff_modulus size: 218 bits
    \ noise_standard_deviation: 3.2

    Number of slots: 4096
    Input vector: 

        [ 0.000, 1.100, 2.200, 3.300 ]

    parms_id of plain: 76ec3b85f92df1fe e8fe24131738ec62 1ce124a10e40ee07 94b6484844e220b2
    parms_id of encrypted: 76ec3b85f92df1fe e8fe24131738ec62 1ce124a10e40ee07 94b6484844e220b2

    Scale in plain: 1.15292e+18
    Scale in encrypted: 1.15292e+18

    Squared input: 

        [ -0.000, 1.210, 4.840, 10.890, ..., -0.000, -0.000, -0.000, -0.000 ]

    Scale in the square: 1.32923e+36 (120 bits)
    Current coeff_modulus size: 218 bits
    Modulus switching ...
    Current coeff_modulus size: 164 bits

    Squared input: 

        [ 0.000, 1.210, 4.840, 10.890, ..., -0.000, 0.000, -0.000, 0.000 ]

    Divided by 3: 

        [ 0.000, 0.403, 1.613, 3.630, ..., -0.000, -0.000, -0.000, 0.000 ]

    Plaintext summand: 

        [ 20.200, 30.300, 40.400, 50.500 ]

    Sum: 

        [ 20.200, 30.703, 42.013, 54.130, ..., -0.000, -0.000, 0.000, -0.000 ]

    Total memory allocated from the current memory pool: 711 MB

    Run example: 7


### Example 7:

    *********************************************
    ********** Example: CKKS Basics II **********
    *********************************************

    / Encryption parameters:
    | scheme: CKKS
    | poly_modulus_degree: 8192
    | coeff_modulus size: 218 bits
    \ noise_standard_deviation: 3.2

    Number of slots: 4096
    Input vector: 

        [ 0.000, 1.100, 2.200, 3.300 ]

    Chain index of (encryption parameters of) encrypted: 3
    Scale in encrypted before squaring: 1.15292e+18
    Scale in encrypted after squaring: 1.32923e+36 (120 bits)
    Current coeff_modulus size: 218 bits

    Rescaling ...

    Chain index of (encryption parameters of) encrypted: 2
    Scale in encrypted: 7.3787e+19 (66 bits)
    Current coeff_modulus size: 164 bits

    Squaring and rescaling ...

    Chain index of (encryption parameters of) encrypted: 1
    Scale in encrypted: 3.02231e+23 (78 bits)
    Current coeff_modulus size: 110 bits

    Rescaling and squaring (no relinearization) ...

    Chain index of (encryption parameters of) encrypted: 0
    Scale in encrypted: 7.03687e+13 (46 bits)
    Current coeff_modulus size: 55 bits

    Eighth powers: 

        [ -0.002, 2.176, 548.848, 14063.983, ..., 0.000, 0.000, 0.000, -0.000 ]

    Precise result: 

        [ 0.000, 2.144, 548.759, 14064.086 ]


    SEAL Examples:

    1. BFV Basics I
    2. BFV Basics II
    3. BFV Basics III
    4. BFV Basics IV
    5. BFV Performance Test
    6. CKKS Basics I
    7. CKKS Basics II
    8. CKKS Basics III
    9. CKKS Performance Test
    0. Exit

    Total memory allocated from the current memory pool: 711 MB

    Run example: 8

### Example 8:

    **********************************************
    ********** Example: CKKS Basics III **********
    **********************************************

    / Encryption parameters:
    | scheme: CKKS
    | poly_modulus_degree: 8192
    | coeff_modulus size: 160 bits
    \ noise_standard_deviation: 3.2

    Number of slots: 4096
    Input vector: 

        [ 0.0000000, 0.0002442, 0.0004884, ..., 0.9995116, 0.9997558, 1.0000000 ]

    Evaluating polynomial PI*x^3 + 0.4x + 1 ...

    Parameters used by all three terms are different:
    Modulus chain index for encrypted_x3: 1
    Modulus chain index for encrypted_x1: 2
    Modulus chain index for plain_coeff0: 3

    Scale in encrypted_x3: 1099500879876.0625000000
    Scale in encrypted_x1: 1099502714881.0000000000
    Scale in plain_coeff0: 1099502714881.0000000000

    Modulus chain index for encrypted_result: 1
    Scale in encrypted_result: 1099502714881.0000000000 (40 bits)
    Result of PI*x^3 + 0.4x + 1:

        [ 1.0000008, 1.0000975, 1.0001954, ..., 4.5367914, 4.5391888, 4.5415872 ]

    Current coeff_modulus size for encrypted_result: 80 bits

    Scale in plain_integer_scalar scale: 1.0000000000
    Scale in encrypted_result: 1099502714881.0000000000
    Result of 7 * (PI*x^3 + 0.4x + 1):

        [ 7.0000054, 7.0006825, 7.0013678, ..., 31.7575397, 31.7743217, 31.7911104 ]

    Result rotated with dbc 15:

        [ 6.9915679, 6.9999223, 7.0023525, ..., 31.7746874, 31.7906100, 7.0001992 ]

    Result rotated with dbc 30:

        [ -60.03267, 154.22515, -71.31642, ..., 25.35366, 37.29308, 21.87917 ]


    SEAL Examples:

    1. BFV Basics I
    2. BFV Basics II
    3. BFV Basics III
    4. BFV Basics IV
    5. BFV Performance Test
    6. CKKS Basics I
    7. CKKS Basics II
    8. CKKS Basics III
    9. CKKS Performance Test
    0. Exit

    Total memory allocated from the current memory pool: 720 MB

