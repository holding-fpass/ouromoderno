# Ouro Moderno
Definições de integração

## Overview
![Overview](https://www.plantuml.com/plantuml/png/rLPHRzis47xNhpYeGE40Y-susR8PorQZMDSCpH8qpWeCVJcB5vkDH6haqOdd-SSKUoZs0_QoL_-naPAjoOcy14YnO81XaBhlkpj-7odnkMKqxB8qOiafGNpYZ8PZBSWe7KSHEjRAPICoKMGlfShHO0R7U3smYPJ7P2sEwJMfCKzWhzlrX8IrWVY5LaoAtwbZL1vXOa0B0v59jKASMxVyOAG6j7xiqu_qDRmy7PoTmPFH_ZD11mUVHxuQoqJcg1ZUqcYGvm7-CaTho-a6Od1wL8Nz-2PxelLbdHxc4Ia1ld9HlRvqUB9tqElkFzlQH02liDjRgO7yawfOD3RQvV0ZJccCou_BttKK983TR-fhwyCWyVesn-fa40I9CrRRUq7HfMZ_hI0Wv6Rv1-GfpZn8O46vWn4Wq4gbkWH3GXfAf5QOaMAz4zKeFiDjxVhmvWOdKfe9z9JCPhq9SsxxdSvStyonw7px_eeFv_vlyTGGEwFEJNeuhuQBfpoXZ0ulGhwetaDJOzwlFln4Hbx8f2Qp8K6A9QRG-k7jS2SgivVQDAziK94zLli0e2utUbCFbRpDihnJvonJ-IigeQ5Q6RH-SMHcK1pEdR3DrbliEyjEwBlQ-vFi2ezsff9xQYEjmz1AjRvy-M4i5KBhmhT1bUlbmVjwT_TMYjEJis5fj97Z2KVp222cAIa-4d4VulcO51ba4kTEYaNyMHKVpd8AY3JiFjMUIhegeuQiTYQXLO8oVXIuzUWclOonpCWoPda8zBgztcxN_mw6Uxt-_XVzRlVdE5e4QI4qlF8TNWs7f_122u9UTwz9jmRX1bDKou-Fh9YYQtweOZcQVmhdXgPIE_kwngymZwht6xAvLeB2aK7m9yMotx3_XTONscHjKvItr4uCUN2Zw06uLV8GR10y_NH25p21Z1uHLkbdXKnIgrTe9qKvFNfF2OVf8YuXgEmLcMs0nQTnpyDLaZfR-ypzZwnB_CpI0smO6nGXKxVzrPVt4jL7w-__uXgs_4je2wr2kqxuJdUGvLGAdxh3P7dzul7IDRteJqPSt092ac31EkK6QJqEnsqiBHkzWTtsUIdOdbvtj0y3hdzPwdcW5QpTUPYtHJR5RPUB06kqTJszmrhu3SgZvWUO15LvveTXKzpRzRgdpP9krwFmkhXMtxewV7_ZFUSVrhCMpEV-psNftm00)

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

## Webhook POST
O formato de webhook pode ser segundo o exemplo abaixo, mas não se limita esse formato, sendo aberto a Ouro Moderno uma sugestão o definição própria.

### Exemplo (sugestivo)
```json
{
  "eventId": "{generatedUuid}",
  "eventType": "form.response.create",
  "resourceId": "{formId}",
  "resourceType": "course",
  "timestamp": "2022-02-07T12:46:00Z",
  "data": {
    "grade": 0.98
  }
}
```

## Campos do Webhook (sugestivo)
- **eventId**: uuid v4 gerado pelo remetente
- **eventType**: Tipificação do evento
- **resourceId**: Identificador do recurso ao qual o evento se refere
- **resourceType**: Tipificação do recurso
- **timestamp**: Data da geração do evento gerada pelo remetente
- _data_: Dados adcionais não definidos (Opcionais)
- _parentId_: Identificador do recurso ao qual o principal pode se referenciar (Opcional)
- _parentType_: Tipificação do recurso. (Opcional)

### Exemplo de Requisição
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
  "eventId": "{eventId}",
  "message": "Event delivered",
  "delivered": true
}
```