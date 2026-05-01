# Web Brute Force (DVWA)

## Descrição

DVWA (Damn Vulnerable Web Application) é uma aplicação web vulnerável projetada para testes de segurança. Neste cenário, foi explorada uma vulnerabilidade de brute force no mecanismo de autenticação.

## Acesso à aplicação

A aplicação DVWA foi acessada no ambiente local através do seguinte endpoint:

URL: http://192.168.56.101/dvwa/login.php

Credenciais padrão:

* admin / password

Observação: o endereço IP é acessível apenas dentro do ambiente virtual configurado (rede local/Host-Only).

## Execução do ataque

Foi utilizada a ferramenta Medusa para automatizar tentativas de autenticação na aplicação DVWA.

Entretanto, devido à ausência de um parâmetro claro de falha na resposta da aplicação, a ferramenta retornou múltiplos resultados como "SUCCESS", caracterizando falsos positivos.

Dessa forma, a validação das credenciais foi realizada manualmente através da interface da aplicação.

## Exemplo de tentativa

Usuário: admin
Senha: 123456

## Resultado

Foi possível realizar múltiplas tentativas de autenticação até obter acesso válido à aplicação.

## Evidências

### Página de ataque

<img width="1654" height="1114" alt="image" src="https://github.com/user-attachments/assets/3c0c868c-098a-4738-972c-bb5922ae42cc" />

### Acesso obtido

<img width="1638" height="1211" alt="image" src="https://github.com/user-attachments/assets/dc5d6310-f5c8-48ce-988a-b3a129f1eb58" />

## Impacto

* Acesso não autorizado à conta administrativa
* Possível comprometimento da aplicação
* Exposição de dados sensíveis

## Mitigação

* Implementação de CAPTCHA
* Rate limiting (limitação de tentativas de login)
* Autenticação multifator (MFA)
* Bloqueio temporário após múltiplas tentativas falhas
* Monitoramento de tentativas suspeitas

## Análise

A vulnerabilidade ocorre devido à ausência de mecanismos de proteção contra brute force, permitindo que um atacante realize múltiplas tentativas de login até encontrar credenciais válidas.
