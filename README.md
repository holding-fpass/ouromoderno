# Ouro Moderno
Definições de integração

## Overview
![Overview](https://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/holding-fpass/ouromoderno/main/uml/overview-v1.0.0.iuml)

## Glossário
Como forma de equiparar estruturas informacionais nos respectivos sistemas, seguem as definições:
- Curso FPASS equivale a Curso Ouro Moderno
- Módulo FPASS equivale a Aula Ouro Moderno
- Aula FPASS equivale a Atividade (ou Modalidade) de Aula Ouro Moderno.

## Acesso do usuário
Identidade do usuário será realizada por JWT, emitida pelo FPASS e verificada pela OuroModerno. O envio será por _query param_ junto a URL.
Chave de verificação (Verify Signature Secret - https://jwt.io) será definida pelo FPASS.

## FPASS API

FPASS disponibilizará OpenAPI para receber informações enviadas pela OuroModerno, sendo:
Webhooks a serem enviados pela OuroModerno -> FPASS:
1. Visualização de página (frame) da aula
2. Avanço de página (frame) da aula
3. Respondendo a questão
4. Conclusão de curso
5. Atualização de conteúdo

## OuroModerno API
OuroModerno disponibilizará OpenAPI para FPASS obter informações, sendo (exemplo mas não limitado a):
- Listagem de cursos (Ex.: GET /course)
- Detalhe de curso (Ex.: GET /course/{id})
- Listagem de aulas (Ex.: GET /lesson)
- Detalhe de aula (Ex.: GET /lesson/{id})

## Fluxos
- Identificação do usuário
Gerar credencial (JWT) do usuário para integração. Resp.: FPASS
- Verificação da credencial. Resp.: OuroModerno
- Aquisição do curso
  - Checkout de compra. Resp.: FPASS
  - Split de valores. Resp.: FPASS
  - Listagem de transações. Resp.: FPASS
  - Transferência de fundos.: Resp.: FPASS
- Navegação de cursos, módulos e aulas ao usuário. Resp.: FPASS
- Atualização de dados de navegação de conteúdos (cursos/módulos/aulas)
  - Envio de Webhook informando que houve mudança. Resp.: OuroModerno
  - Atualizar dados referente de navegação. Resp.: FPASS
- Apresentação da aula ao usuário
  - Redirecionado para URL do player de aula externo da OuroModerno. Resp.: FPASS
  - Definição da URL de direcionamento. Resp.: OuroModerno
  - Retorno a WebApp FPASS. Resp.: OuroModerno
- Apresentação do Certificado.
  - Informação quanto a conclusão da aula por webhook. Resp.: OuroModerno
  - Emissão do certificado de conclusão do curso.: Resp.: FPASS

Webhook POST - Exemplo
```sh
curl --location -g --request POST 'https://{webhook}' \
--header 'x-api-key: {secretUuid}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "key": "value"
}'
```

Response (201)
```json
{
  "timestamp": "2022-07-02T12:32:00Z"
}
```