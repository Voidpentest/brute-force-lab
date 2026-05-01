# Reconnaissance (Nmap)

## Descrição

A fase de reconhecimento tem como objetivo identificar serviços ativos na máquina alvo, permitindo mapear possíveis vetores de ataque.

## Comando utilizado

```bash id="2z0s0o"
nmap -sV -p 21,22,80,445,139 192.168.56.101
```

## Resultados

| Porta | Serviço | Versão               |
| ----- | ------- | -------------------- |
| 21    | FTP     | vsftpd 2.3.4         |
| 22    | SSH     | OpenSSH 4.7p1        |
| 80    | HTTP    | Apache httpd 2.2.8   |
| 139   | SMB     | Samba smbd 3.x - 4.x |
| 445   | SMB     | Samba smbd 3.x - 4.x |


## Evidência

<img width="760" height="278" alt="image" src="https://github.com/user-attachments/assets/73c65832-f0df-406b-8697-2b23ee48dd12" />

## Análise

O scan identificou múltiplos serviços expostos na máquina alvo, aumentando a superfície de ataque.

Os principais vetores identificados foram:

* FTP (porta 21) → suscetível a brute force
* HTTP (porta 80) → aplicação web vulnerável (DVWA)
* SMB (portas 139 e 445) → possível enumeração e exploração de compartilhamentos

## Observação

O endereço IP é acessível apenas dentro do ambiente virtual configurado (rede local/Host-Only).
