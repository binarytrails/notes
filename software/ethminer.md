# ethminer

## compiling

### prereq windows

    echo "Powershell must be run as Administrator for this command to work"
    Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

    choco install -y git
    $Env:path += ";C:\Program Files\Git\bin"

    choco install -y visualstudio2019community
    echo ".NET core and build tools"
    choco install visualstudio2019buildtools --package-parameters "--allWorkloads --includeRecommended --includeOptional --passive --locale en-US"
    $Env:path += ";C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools\Common7\IDE\CommonExtensions\Microsoft\CMake\CMake\bin"

    mkdir /tools
    choco install strawberryperl
    $Env:path += ";C:\tools\Strawberry\perl\bin"

### prereq linux

    # arch linux
    pacman -S nvidia cuda

### compile latest greatest

    git clone https://github.com/ethereum-mining/ethminer
    cd ethminer
    git submodule update --init --recursive

    # fix broken boost link: https://github.com/ethereum-mining/ethminer/issues/2290 
    git fetch origin 47348022be371df97ed1d8535bcb3969a085f60a
    git cherry-pick 47348022be371df97ed1d8535bcb3969a085f60a

## build the executable binary

    mkdir ethminer/build
    cd ethminer/build

    # compile the project (you can add more flags here -D<flag>)
    cmake ..

    # build with make (linux compatible)
    make -j24 # -j<number-of-cpu-threads>

    # build with cmake (windows compatible)
    cmake --build . --config Release --target package

    # install (linux compatible)
    cmake --install . --prefix /usr --config Release

## running ethminer

    # HWMON (monitor heat), tstart (suspend after degrees), tstop (stop after degrees), etc.
    ethminer -P stratum://0x<miner-addr>.<miner-name>@us1.ethermine.org:4444 -v 3 \
        --HWMON 1 --tstart 80 --tstop 85 \
        --work-timeout 600 --farm-retries 10 --response-timeout 10

## monitoring

    # heat level, processes using it, capacity being used
    nvidia-smi

    # network connections
    watch ss -ltpa

