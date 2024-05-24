1. Crear una maquina virtual en el vagrant file.

 config.vm.define :pertoper do |pertoper|
 pertoper.vm.box = "bento/ubuntu-22.04"
 pertoper.vm.network :private_network, ip: "192.168.50.9"
 pertoper.vm.hostname = "pertoper"
 end

2. Descargar el aplicativo de ethereum para Ubuntu a 64 bits, haciendo las instalaciones pertinentes con los siguientes comandos.
 sudo apt-get update
 sudo apt-get install software-properties-common
 sudo apt-get install -y ethereum
 sudo add-apt-repository -y ppa:ethereum/ethereum

 3. Revisamos la correcta instalación con su respetiva version.
 geth version

 4. Creamos el directorio, donde será nuestro espacio de trabajo y el directorio donde se almacenará los datos del nodo.
 mkdir ethereum-local.net

5. Entramos a la carpeta creada para crear un directorio del nodo 1.
 cd ethereum-local.net

6. Creamos el archivo genesis.json
 {
"config": {
        "chainId": 4777,
        "homesteadBlock": 0,
        "eip150Block": 0,
        "eip155Block": 0,
        "eip158Block": 0
        },
"difficulty": "2",
"gasLimit": "2100000",
"alloc": {
        "0xB65E7DB245b858aFf3c08757CdC3B100d74300f5": {
                "balance": "30000000000000000"
        }
        }
}

7. Creamos la carpeta node-1 y si mismo el nodo 1
mkdir node-1
geth -datadir node-1 account new

8. Editamos el archivo genesis.json cambiando la llave publica del nodo 1
Public address of the key:   0xbDAeaFb0a192e6055cB77D0278D67b020f33a98f
Path of the secret key file: node-1/keystore/UTC--2024-05-21T03-11-40.932669566Z--bdaeafb0a192e6055cb77d0278d67b020f33a98f
{
    "config": {
        "chainId": 4777,
        "homesteadBlock": 0,
        "eip150Block": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
    "difficulty" : "2",
    "gasLimit" : "2100000",
    "alloc" :{
        "0xf189ec0a4A111Df655cA4c11e5D340f820BD07F5": {
            "balance": "10000000"}
    }
}

9. Actulizamos el nodo 1 con el archivo genesis.json
geth --identity node-1 init genesis.json --datadir node-1

10. Iniciamos el nodo 1
geth --datadir node-1  --maxpeers 2 --networkid 13 --http -- http.addr “0.0.0.0” --http-api 8545 -- port 30301 --allow-insecure-unclock

"De esta forma creamos el resto de nodos"

11. Revisamos la correcta conexion con el siguiente comando.
geth attach /home/usuario/desktop/ethereum-local.net/node-1/geth.ipc