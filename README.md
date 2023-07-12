# Aleo
Aleo deploying smart contract
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
rustup install stable
rustup update stable
rustup update stable
git clone https://github.com/AleoHQ/leo
cd leo
apt install clang gcc libssl-dev pkg-config
cargo install --path .
git clone https://github.com/AleoHQ/snarkOS.git --depth 1
cd snarkOS
./build_ubuntu.sh
cargo install --path .
mkdir demo_deploy_Leo_app && cd demo_deploy_Leo_app
WALLETADDRESS="aleo1nnrxyjp4kmjvew04fcpt4ss8a8gc2cq3auxspehhf0gau92rdcpswl3f00"
APPNAME=helloworld_"${WALLETADDRESS:4:6}"
leo new "${APPNAME}"
PATHTOAPP=$(realpath -q $APPNAME)
cd $PATHTOAPP && cd ..
PRIVATEKEY="APrivateKey1zkp2WAieamLGcFv9htET23mvi3BEH3oEe135HkABh4WWAVu"
RECORD="{
  owner: aleo1j7qxyunfldj2lp8hsvy7mw5k8zaqgjfyr72x2gh3x4ewgae8v5gscf5jh3.private,
  microcredits: 1500000000000000u64.private,
  _nonce: 3077450429259593211617823051143573281856129402760267155982965992208217472983group.public
}"
snarkos developer deploy "${APPNAME}.aleo" --private-key "${PRIVATEKEY}" --query "https://vm.aleo.org/api" --path "./${APPNAME}/build/" --broadcast "https://vm.aleo.org/api/testnet3/transaction/broadcast" --fee 600000 --record "${RECORD}"
