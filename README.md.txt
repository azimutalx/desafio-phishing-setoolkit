# Credential Harvester com SET — laboratório de engenharia social

> **Aviso.** Este repositório é material de estudo do bootcamp da DIO. Todo o
> procedimento foi executado em **rede local isolada**, capturando apenas
> credenciais de teste digitadas por mim. Clonar páginas e capturar senhas de
> terceiros é crime no Brasil (art. 154-A do Código Penal e furto mediante
> fraude). O objetivo aqui é **entender o ataque para defender**.

## Objetivo
Compreender como funciona um ataque de *credential harvesting* por engenharia
social, usando o Social-Engineer Toolkit (SET) do Kali Linux, e a partir disso
derivar as defesas que realmente barram esse ataque.

## Ambiente
- Kali Linux (VM), rede em modo *host-only* — sem saída para a internet.
- Alvo: uma segunda VM na mesma rede, operada por mim.
- SET (setoolkit), já incluso no Kali.

## O ataque, em alto nível
O SET oferece o módulo *Credential Harvester* com *Site Cloner*: ele copia a
página de login de um site e sobe um servidor local que serve essa cópia. A
vítima que digita usuário e senha na página falsa tem esses dados gravados em
texto, e normalmente é redirecionada para o site verdadeiro — sem perceber.

O fluxo de menus do SET e cada tela estão documentados nos prints em `/images`.

## O que eu observei (e por que importa)
- **O clone do Facebook falhou** ("Unable to clone this specific site"). Sites
  grandes usam proteções (tokens dinâmicos, JS, cabeçalhos) que quebram a cópia
  estática — ver `images/fbX.png`. Isso já mostra que o ataque é mais frágil do
  que parece em vídeo.
- A página servida fica em **HTTP e num IP local**, nunca no domínio real. A
  barra de endereço do navegador é o ponto onde o ataque se denuncia.
- Sem um segundo fator, uma senha capturada basta. Com 2FA, não.

## Por que funciona — e como defender
| O que o ataque explora | Defesa que o neutraliza |
| --- | --- |
| A vítima não confere a URL | Checar o domínio; usar gerenciador de senhas (ele não preenche em domínio falso) |
| Página sem HTTPS válido | Desconfiar de cadeado ausente / aviso de certificado |
| Senha única protege a conta | **2FA / MFA** — a senha sozinha deixa de bastar |
| Link chega por mensagem/e-mail | Não clicar; digitar o endereço à mão; treinar o usuário |
| Reuso de senha entre sites | Senha única por serviço + gerenciador |

## Conclusão
A engenharia social ataca a pessoa, não o sistema. A defesa mais eficaz é a
combinação de **MFA + hábito de conferir a URL + gerenciador de senhas**, apoiada
em conscientização. Reproduzir o ataque em laboratório serviu para enxergar
exatamente onde essas defesas entram.

## Referências
- Documentação do Social-Engineer Toolkit (TrustedSec)
- Bootcamp DIO — módulo de engenharia social
