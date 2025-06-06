# Modo Magic do CyberChef
## Descrição
Tenta adivinhar qual(is) algoritmo(s) foi(ram) usado(s) no texto codificado/cifrado e retorna as possíveis strings em texto claro. Pode demorar para executar dependendo do Depth definido.

[Link](https://gchq.github.io/CyberChef/#recipe=Magic(3,true,false,'')&input=Wm14aFozdGhZVGRqWmpCaU5qY3dORE13Tm1ObU9EZzNPV1poTldaa016TXdPV1JqTjMwSw)

# Encoding (codificação)
## Finalidade
Usado para transmitir dados (por exemplo, uma imagem) por meio de uma string. Também usado para representar dados de outra forma.

## Base64
Pelo terminal:
```bash
echo 'SGVsbG8gV29ybGQK' | base64 -d
```

[Pelo CyberChef](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)&input=U0dWc2JHOGdWMjl5YkdRSw)

## Base32
Pelo terminal:
```bash
echo 'JBSWY3DPEBLW64TMMQFA====' | base32 -d
```

[Pelo CyberChef](https://gchq.github.io/CyberChef/#recipe=From_Base32('A-Z2-7%3D',true)&input=SkJTV1kzRFBFQkxXNjRUTU1RRkE9PT09)

## Hexadecimal
Pelo terminal:
```bash
echo '48656c6c6f20576f726c640a' | xxd -p -r
```

[Pelo CyberChef](https://gchq.github.io/CyberChef/#recipe=From_Hex('Auto')&input=NDg2NTZjNmM2ZjIwNTc2ZjcyNmM2NDBh)

## ASCII
### Exemplo
72 101 108 108 111 32 87 111 114 108 100

[Pelo CyberChef](https://gchq.github.io/CyberChef/#recipe=From_Decimal('Space',false)&input=NzIgMTAxIDEwOCAxMDggMTExIDMyIDg3IDExMSAxMTQgMTA4IDEwMA)

## Binário
### Exemplo
01001000 01100101 01101100 01101100 01101111 00100000 01010111 01101111 01110010 01101100 01100100

[Pelo CyberChef](https://gchq.github.io/CyberChef/#recipe=To_Binary('Space',8)&input=SGVsbG8gV29ybGQ)

## Esab46
### Descrição
É uma variação do base64, com alfabeto diferente.

### Exemplo
ys1c7sGVvTBK7n6cMA//

[Ferramenta online](http://qbarbe.free.fr/crypto/eng_esab46c.php)

## Código Morse
### Exemplos
.... . .-.. .-.. --- / .-- --- .-. .-.. -..  
(Áudios com bipes curtos e longos. Os bipes curtos representam os pontos e os bipes longos representam os hífens.)

[Pelo CyberChef](https://gchq.github.io/CyberChef/#recipe=From_Morse_Code('Space','Line%20feed')&input=Li4uLiAuIC4tLi4gLi0uLiAtLS0gLyAuLS0gLS0tIC4tLiAuLS4uIC0uLg)

# Hashing
## Definição e finalidade
Função matemática unidirecional que produz uma saída de tamanho fixo. Usada para armazenar senhas de forma segura e verificação de integridade.

## Identificando o algoritmo usado para calcular a hash
[Ferramenta online](https://www.onlinehashcrack.com/hash-identification.php)

## MD5, SHA1, SHA256, LM, NTLM, etc. (algoritmos sem [sal criptográfico](https://pt.wikipedia.org/wiki/Sal_(criptografia)))
### Definição e finalidade do sal criptográfico
Seja a mensagem em texto claro "123456". Antes de fazer o hashing dessa mensagem, podemos concatenar uma string pseudoaleatória, como "2d7d6fca1a". Chamamos essa string pseudaletória de sal criptográfico.

O sal criptográfico é usado para dificultar a quebra das hashes caso elas sejam comprometidas.

### Exemplo de hash MD5 (16 bytes)
e10adc3949ba59abbe56e057f20f883e

### Exemplo de hash SHA1 (20 bytes)
7c4a8d09ca3762af61e59520943dc26494f8941b

### Exemplo de hash SHA256 (64 bytes)
8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92

### Exemplo de hash LM/NTLM (16 bytes)
32ed87bdb5fdc5e9cba88547376818d4

[Ferramenta online](https://crackstation.net/)  
[Ferramenta online 2](https://hashes.com/en/decrypt/hash)

## MD5Crypt, DESCrypt, YesCrypt, BCrypt, etc. (algoritmos com sal criptográfico)
Instalando o John the Ripper (JtR) no Linux:
```bash
apt update; apt install john
```

Armazenando a(s) hash(es) no arquivo hash.txt:
```bash
# 1: algoritmo usado (MD5Crypt)
# teste: sal criptográfico
# 4THtaluAMKEaR3kX2aEH1.: hash propriamente dita
# Mais detalhes em https://www.cyberciti.biz/faq/understanding-etcshadow-file/
echo -e '$1$teste$4THtaluAMKEaR3kX2aEH1.' > hash.txt
```

Baixando a wordlist rockyou.txt (134M):
```bash
curl -OL 'https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt'
```

Utilizando a ferramenta:
```bash
# hash.txt: arquivo com a(s) hash(es) a ser(em) quebrad(as)
# rockyou.txt: arquivo com strings em texto claro; para cada string desse arquivo, o JtR irá calcular a hash e
#              comparar com a(s) hash(es) contida(s) no arquivo hash.txt; se a hash calculada for idêntica a
#              uma das hashes do arquivo hash.txt, dizemos que essa senha correspondente à hash do arquivo
#              hash.txt foi "quebrada"
john hash.txt --wordlist=rockyou.txt
```

Extraindo a(s) senha(s) recuperada(s):
```bash
john --show hash.txt
```

## Quebrando senhas de arquivos .zip protegidos por senha
Extraindo a hash da senha do arquivo flag.zip para um arquivo:
```bash
# A ferramenta zip2john faz parte do pacote John the Ripper
zip2john flag.zip > zip.hash
```

Fazendo brute-force na hash obtida:
```bash
john zip.hash --wordlist=rockyou.txt
```

Extraindo a senha recuperada:
```bash
john --show zip.hash
```

## Quebrando senhas de chaves SSH privadas protegidas por senha (passphrase)
Extraindo a hash da senha do arquivo id_rsa para um arquivo:
```bash
# A ferramenta ssh2john faz parte do pacote John the Ripper
ssh2john id_rsa > ssh.hash
```

Fazendo brute-force na hash obtida:
```bash
john ssh.hash --wordlist=rockyou.txt
```

Extraindo a senha recuperada:
```bash
john --show ssh.hash
```
## Quebrando senhas de usuários Linux
Extraindo a(s) hash(es) do(s) usuário(s) para um arquivo:
```bash
# A ferramenta unshadow faz parte do pacote John the Ripper
unshadow /etc/passwd /etc/shadow | grep ':\$' > hashes.txt
```

Fazendo brute-force na(s) hash(es) obtida(s):
```bash
john hashes.txt --wordlist=rockyou.txt --format=crypt
```

Extraindo a(s) senha(s) recuperada(s):
```bash
john --show hashes.txt
```

# Criptografia
## Definição
Uma maneira de transmitir dados de forma segura sem que terceiros consigam descobrir o que está sendo trafegado. Ao contrário dos algoritmos de hashing, os algoritmos de criptografia são bidirecionais, em que sempre será possível obter a mensagem em texto claro se conhecermos o algoritmo e os dados usados para descriptografar a mensagem (chave, IV, etc).  

Os algoritmos podem ser de chave simétrica ou de chave assimétrica. Nos algoritmos de chave simétrica, a chave para criptografar uma mensagem em texto claro é a mesma chave para descriptografar o texto cifrado. Já nos algoritmos de criptografia assimétrica, a chave para criptografar uma mensagem em texto claro, chamada de chave pública, é diferente da chave para descriptografar o texto cifrado, chamada de chave privada.

## ROT13
### Descrição e classificação
Também conhecido como [Cifra de César](https://pt.wikipedia.org/wiki/Cifra_de_C%C3%A9sar). É um algoritmo de criptografia simétrica.

### Exemplo
Uryyb Jbeyq

[Pelo CyberChef](https://gchq.github.io/CyberChef/#recipe=ROT13_Brute_Force(true,true,false,100,0,true,'')&input=VXJ5eWIgSmJleXE)

## Cifra maçônica
### Descrição
Também conhecida como [cifra pigpen](https://pt.wikipedia.org/wiki/Cifra_ma%C3%A7%C3%B3nica). É um algoritmo de substituição em que as letras do alfabeto são substituídas por símbolos.

![Letras do alfabeto e seus respectivos símbolos na cifra maçônica](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*4tHgmNenIr6KT586F03CCg.jpeg)

### Exemplo
![Exemplo de mensagem criptografada usando a cifra maçônica](https://upload.wikimedia.org/wikipedia/commons/thumb/b/ba/A-pigpen-message.svg/1920px-A-pigpen-message.svg.png)

[Ferramenta online](https://www.dcode.fr/pigpen-cipher)

## Atbash
### Definição
Outro algoritmo de substituição em que a primeira letra do alfabeto é substituída pela última, a segunda é substituída pela penúltima, etc.

### Exemplo
Svool Dliow

[Pelo CyberChef](https://gchq.github.io/CyberChef/#recipe=Atbash_Cipher()&input=U3Zvb2wgRGxpb3c&ieol=CRLF&oeol=CRLF)

## XOR
### Classificação
Algoritmo de criptografia simétrica.

### Exemplo
Xy97JnhqQCVlJnM=

[Pelo CyberChef]([https://gchq.github.io/CyberChef/#recipe=XOR_Brute_Force(2,100,0,'Standard',false,true,false,'Hello')&input=Xy97JnhqQCVlJnM](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)XOR_Brute_Force(2,100,0,'Standard',true,true,false,'Hello')&input=WHk5N0puaHFRQ1ZsSm5NPQ&ieol=CRLF))

### Observações
* Às vezes é necessário decodificar a string em base64 antes de fazer o XOR.
* É possível obter a chave (ou parte da chave) que foi usada para criptografar a mensagem em texto claro se conhecermos parte da mensagem em texto claro. Por exemplo:  
  
    - Mensagem em texto claro: `flag{teste}`  
    - Chave de criptografia: `chave`  
    - Texto cifrado, em base64: `XOR(flag{teste}, chave) = BQQAER4XDRICAB4=`  
        [Reproduzir no CyberChef](https://gchq.github.io/CyberChef/#recipe=XOR(%7B'option':'Latin1','string':'chave'%7D,'Standard',false)To_Base64('A-Za-z0-9%2B/%3D')&input=ZmxhZ3t0ZXN0ZX0)

    Recuperando a chave usada para criptografar a flag: `XOR(flag{, BQQAER4XDRICAB4=) = chave`  
    [Reproduzir no CyberChef](https://gchq.github.io/CyberChef/#recipe=XOR(%7B'option':'Base64','string':'BQQAER4HABAGAAk%3D'%7D,'Standard',false)&input=ZmxhZ3s&oeol=CR)
* A chave de criptografia pode ser uma string (QualquerCoisa), um número inteiro (42) ou um conjunto de bytes em hexadecimal (deadbeef).

## AES
### Classificação
Algoritmo de criptografia simétrica.

### Exemplos
x2SnOeNky3TIziCuI1Lw8Q==  
dAJrw51l01FbsZbmMKB+cw==

[Pelo CyberChef (modo CBC)](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)AES_Decrypt(%7B'option':'Latin1','string':'0123456789abcdef'%7D,%7B'option':'Latin1','string':'0123456789abcdef'%7D,'CBC','Raw','Raw',%7B'option':'Hex','string':''%7D,%7B'option':'Hex','string':''%7D)&input=eDJTbk9lTmt5M1RJemlDdUkxTHc4UT09)  
[Pelo CyberChef (modo ECB)](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)AES_Decrypt(%7B'option':'Hex','string':'59177a797d6efa491efe76d7f0b5cc59177a797d6efa491efe76d7f0b5cc4200'%7D,%7B'option':'Hex','string':''%7D,'ECB','Raw','Raw',%7B'option':'Hex','string':''%7D,%7B'option':'Hex','string':''%7D)&input=ZEFKcnc1MWwwMUZic1pibU1LQitjdz09)

### Observações
* Às vezes é necessário decodificar a string em base64 antes de descriptografar a string usando AES.
* O modo ECB não usa de vetor de inicialização (IV).

## Fernet
### Descrição
Algoritmo de criptografia simétrica. É comum o texto cifrado começar com a string "gAAAAA".

### Exemplo
gAAAAABoPJwOD4mAdUCn0y_nxuGTuYPMGxTSM9L90JTX_KdcVla-4c8iovzbLZOJXz3DUdERnhIUCliDWTXsoahbkH-OAg68Yw==

[Pelo CyberChef](https://gchq.github.io/CyberChef/#recipe=Fernet_Decrypt('ZDY5Y2U1ZDY0NmM1ZTI5M2IwNjM1ZjVhNmM4MzJiOTE%3D')&input=Z0FBQUFBQm9QSndPRDRtQWRVQ24weV9ueHVHVHVZUE1HeFRTTTlMOTBKVFhfS2RjVmxhLTRjOGlvdnpiTFpPSlh6M0RVZEVSbmhJVUNsaURXVFhzb2FoYmtILU9BZzY4WXc9PQ)

## RSA
### Descrição
Algoritmo de criptografia assimétrico. A ferramenta RsaCtfTool é útil para encontrar vulnerabilidades comuns quando as chaves RSA são geradas de maneira insegura.

[Repositório da ferramenta RsaCtfTool](https://github.com/RsaCtfTool/RsaCtfTool)

Exemplo de utilização da ferramenta:
```bash
./RsaCtfTool.py -n <valor de n> -e <valor de e> --uncipher <texto cifrado codificado em hexadecimal>
```

## Vigenère
### Exemplo
PayeoEkpuz

[Pelo CyberChef](https://gchq.github.io/CyberChef/#recipe=Vigen%C3%A8re_Encode('SenhaSecretaDeExemplo')&input=UGF5ZW9Fa3B1eg)

# Esteganografia
## Definição
Diferente da criptografia, na esteganografia tentamos ocultar a existência da mensagem.

## Ferramentas
### strings
Este utilitário permite visualizar as strings de um arquivo.

Utilização da ferramenta:
```bash
string <arquivo>
```

### exiftool
Esta ferramenta permite visualizar metadados de um arquivo.

Instalação da ferramenta:
```bash
apt install exiftool
```

Utilização da ferramenta:
```bash
exiftool <arquivo>
```

### zsteg
Esta ferramenta permite extrair mensagens ocultas de arquivos .bmp e .png.

Instalação da ferramenta:
```bash
gem install zsteg
```

Utilização da ferramenta:
```bash
zsteg -a <arquivo .bmp ou .png>
```

[Repositório da ferramenta zsteg](https://github.com/zed-0xff/zsteg)

### binwalk
Esta ferramenta permite extrair arquivos ocultos de outros arquivos.

Instalação da ferramenta:
```bash
apt update; apt install python3-binwalk
```

Utilização da ferramenta:
```bash
python3 -m binwalk -Me <arquivo>
```

Após a execução do comando acima, um diretório com o(s) arquivo(s) extraído(s) será criado. O nome desse diretório termina com a string "extracted".

[Repositório da ferramenta binwalk](https://github.com/ReFirmLabs/binwalk)

### stegseek
Esta ferramenta permite extrair arquivos ocultos de arquivos .jpg e .wav por meio de uma wordlist com senhas.

Instalação da ferramenta:
```bash
apt update; apt install stegseek
```

Utilização da ferramenta:
```bash
# A wordlist com senhas normalmente é a rockyou.txt. No entanto, a senha pode ser encontrada durante algum passo anterior do desafio
stegseek <arquivo .jpg ou .wav> <wordlist com senhas>
```

[Repositório da ferramenta stegseek](https://github.com/RickdeJager/StegSeek)

### stegsolve
Esta ferramenta permite aplicar filtros em imagens, possivelmente revelando segredos nelas.

Download da ferramenta:
```bash
curl -OL 'https://github.com/Giotino/stegsolve/releases/download/v1.4/StegSolve-1.4.jar'
```

Utilização da ferramenta:
```bash
java -jar StegSolve-1.4.jar
```

### Esteganografia em áudio
Caso o desafio forneça um arquivo em áudio, e esse áudio apresente chiado, possivelmente há alguma mensagem oculta nele.  

Esta [ferramenta online](https://academo.org/demos/spectrum-analyzer/) permite analisar o espectro de áudio e revelar possíveis strings ocultas. Alternativamente, podemos baixar e utilizar o programa Audacity.
