# Brute Force Lab

## Objetivo

Este projeto tem como objetivo simular ataques de **brute force** em um ambiente controlado utilizando Kali Linux como máquina atacante e Metasploitable 2 como alvo, visando identificar vulnerabilidades relacionadas a autenticação fraca e demonstrar possíveis medidas de mitigação.

## Ambiente

| Máquina          | Função   | IP     |
| ---------------- | -------- | ------ |
| Kali Linux       | Atacante | 192.168.56.102 |
| Metasploitable 2 | Alvo     | 192.168.56.101 |

## Metodologia

O projeto foi dividido nas seguintes etapas:

1. Reconhecimento (Nmap)
2. Enumeração de serviços
3. Execução de ataques de brute force
4. Validação de acesso
5. Análise de impacto
6. Proposta de mitigação

## Reconhecimento

Foi utilizado o Nmap para identificar os serviços ativos na máquina alvo:

```bash
nmap -sV -p 21,22,80,445,139 192.168.56.101
```

### Serviços identificados:

* FTP (porta 21)
* SSH (porta 22)
* HTTP (porta 80)
* SMB (portas 139 e 445)

## Ataque 1 — FTP Brute Force

### Ferramenta:

Medusa

### Comando utilizado:

```bash
medusa -h 192.168.56.101 -U users.txt -P passwords.txt -M ftp -t 6
```

### Resultado:

Credenciais encontradas:

* Usuário: msfadmin
* Senha: msfadmin

### Validação:

```bash
ftp 192.168.56.101
```

### Impacto:

Acesso não autorizado ao servidor FTP, permitindo leitura e modificação de arquivos.

### Mitigação:

 * Uso de senhas fortes
 * Bloqueio após múltiplas tentativas (rate limiting)
 * Implementação de autenticação multifator (MFA)
 * Monitoramento de tentativas de login
 * Políticas de senha seguras (evitar credenciais fracas e reutilização)

## Ataque 2 — Web Brute Force (DVWA)

### Acesso:

A aplicação DVWA foi acessada no ambiente local através do seguinte endpoint:

URL: http://192.168.56.101/dvwa/login.php

Observação: o endereço IP é acessível apenas dentro do ambiente virtual configurado (rede local/Host-Only).

### Credenciais padrão:

* admin / password

### Exploração:

Foi utilizado o módulo de brute force do DVWA com nível de segurança configurado como **LOW**, permitindo múltiplas tentativas de login sem restrições.

### Resultado:

Login realizado com sucesso.

### Impacto:

Comprometimento da conta administrativa da aplicação web.

### Mitigação:

 * Implementação de CAPTCHA
 * Rate limiting (limitação de tentativas de login)
 * Autenticação multifator (MFA)
 * Bloqueio temporário após múltiplas tentativas falhas
 * Monitoramento de tentativas de login suspeitas 

## Ataque 3 — SMB (Enumeração e Password Spraying)

### Enumeração:

```bash
enum4linux -a 192.168.56.101 | tee enum4_output.txt
```

### Ataque:

```bash
medusa -h 192.168.56.101 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 4
```

### Validação:

```bash
smbclient -L //192.168.56.101 -U user
```

### Impacto:

Possível acesso a compartilhamentos de arquivos e vazamento de informações sensíveis.

### Mitigação:

 * Restrição de acesso a compartilhamentos SMB (controle de permissões)
 * Uso de senhas fortes e políticas de autenticação seguras
 * Desativação de acessos anônimos (anonymous login)
 * Monitoramento de tentativas de autenticação
 * Limitação de tentativas de login (rate limiting / lockout)
 * Desativação de versões inseguras do SMB (ex: SMBv1)


## Técnicas Utilizadas

* Brute Force Attack
* Password Spraying
* Service Enumeration
* Credential Testing

## Principais Vulnerabilidades Identificadas

* Uso de senhas fracas
* Falta de proteção contra brute force
* Credenciais padrão expostas

## Aprendizados

* A importância de proteger sistemas contra tentativas automatizadas
* Como serviços expostos aumentam a superfície de ataque
* A necessidade de monitoramento e boas práticas de autenticação

## Aviso

Este projeto foi realizado exclusivamente para fins educacionais em ambiente controlado.
Não utilize essas técnicas em sistemas reais sem autorização.
