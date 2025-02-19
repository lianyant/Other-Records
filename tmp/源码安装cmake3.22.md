```
sudo apt update
sudo apt install build-essential 
sudo apt install libssl-dev
export OPENSSL_ROOT_DIR=/usr/include/openssl
sudo apt install pkg-config
wget https://cmake.org/files/v3.31/cmake-3.31.5.tar.gz
tar -xzvf cmake-3.31.5.tar.gz
cd cmake-3.31.5
./bootstrap
make -j$(nproc)
sudo make install
# Update PATH Environment Variable
which cmake
/usr/local/bin/cmake
export PATH=/usr/local/bin/cmake:$PATH
source ~/.bashrc
cmake --version



```
