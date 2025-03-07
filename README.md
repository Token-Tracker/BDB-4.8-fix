# Solution: Install Berkeley DB 4.8 Properly

Since you've already downloaded and extracted Berkeley DB, follow these exact steps to ensure it gets correctly installed and linked.

## 1. Manually Install Berkeley DB 4.8
```sh
cd ~
wget http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz
tar -xvf db-4.8.30.NC.tar.gz
cd db-4.8.30.NC/build_unix
```

## 2. Configure and Build
```sh
../dist/configure --enable-cxx --disable-shared --with-pic --prefix=/usr/local
make -j$(nproc)
sudo make install
```

## 3. Ensure the System Finds the Library
```sh
sudo ln -sf /usr/local/include/db4.8 /usr/include/db4.8
sudo ln -sf /usr/local/lib/libdb4.8.so /usr/lib/libdb4.8.so
sudo ln -sf /usr/local/lib/libdb4.8.a /usr/lib/libdb4.8.a
```

## 4. Retry Tokentrackercoin Core Configuration
Now go back to the Tokentrackercoin source folder and run:

```sh
cd ~/tokentrackercoin
make clean  # Ensure previous build artifacts are removed
./autogen.sh
export BDB_PREFIX='/usr/local'
./configure --with-incompatible-bdb --with-libdb-prefix=$BDB_PREFIX
```

## Verification: Check if Berkeley DB is Installed
If the error persists, check if `libdb_cxx` exists:

```sh
ls /usr/local/include/db4.8/
```
You should see files like:
```sh
db_cxx.h  db.h
```
If `db_cxx.h` is missing, the Berkeley DB installation failed.
