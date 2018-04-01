---
title: Referência da API

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://kcep.auth.us-east-2.amazoncognito.com/signup?redirect_uri=https%3A%2F%2Fkcep.run&response_type=code&client_id=5hb0vho57tjnds23838up09mm1&state=c9NwnG47RDANkENv6MXtwcwyrM5PpQYO&scope=email%20profile%20openid'>Registre-se para obter uma chave de desenvolvimento</a>
  - <a href='https://github.com/lord/slate'>Documentação impulsionada por Slate</a>

search: false
---

# Introdução

O kcep é um serviço de cache. Toda vez que você utiliza nossas APIs, se não tivermos a informação que você precisa,
consultamos diretamente os Correios e mantemos uma cópia das informações em nossa base. Também atualizamos
periodicamente os registros que já conhecemos, para você não consumir informação desatualizada.

Construímos nosso serviço utilizando tecnologias de ponta para entregar alta disponibilidade, escalabilidade e muita
rapidez, requisitos fundamentais para um serviço nos dias de hoje.

Você encontrará APIs para consultar CEP, preços e prazos. Se tiver alguma crítica ou sugestão entre em
[contato](kceprun@yahoo.com).

No momento nossos serviços estão gratuitos e continuarão assim até 20 de Janeiro de 2019. Nesse meio tempo, você pode
contribuir pagando uma cerveja ao @kdiogenes com o [PicPay](https://www.picpay.com/site/). ;)

# Autenticação

> Para autorizar, utilize esse código:

```shell
# Em um shell, você pode passar o cabeçalho correto para cada requisição
curl "https://api.kcep.run/85801-001"
  -H "x-api-key: DyYOx4YvF5uCrJGWkxPy7wDYNcansaq8VTo4unX8"
```

Utilizamos chaves para conceder acesso a nossa API. Você pode registrar um nova chave fazendo
[registro](https://kcep.auth.us-east-2.amazoncognito.com/signup?redirect_uri=https%3A%2F%2Fkcep.run&response_type=code&client_id=5hb0vho57tjnds23838up09mm1&state=c9NwnG47RDANkENv6MXtwcwyrM5PpQYO&scope=email%20profile%20openid)
em nosso sistema.

Todas as chamadas a nossa API devem incluir o seguinte cabeçalho:

`x-api-key: DyYOx4YvF5uCrJGWkxPy7wDYNcansaq8VTo4unX8`

<aside class="notice">
Você deve substituir <code>DyYOx4YvF5uCrJGWkxPy7wDYNcansaq8VTo4unX8</code> por sua própria chave de API, pois a
utilizada nesse documento possui limitações.
</aside>

# CEPs, Preços e Prazos

## Obter informações de um CEP

```shell
curl "https://api.kcep.run/85801-001"
  -H "x-api-key: DyYOx4YvF5uCrJGWkxPy7wDYNcansaq8VTo4unX8"
```

> O comando acima irá retornar uma estrutura JSON semelhante a esta:

```json
{
  "complemento2":"- de 6871 a 7543 - lado ímpar",
  "uf":"PR",
  "cidade":"Cascavel",
  "atualizado_em":"2018-04-01T19:25:03.961Z",
  "complemento":null,
  "bairro":"Centro",
  "end":"Avenida Brasil",
  "id":"0",
  "cep":"85801001"
}
```

### Requisição HTTP

`GET https://api.kcep.run/<cep>`

### Parâmetros de Consulta

Parâmetro | Descrição
--------- | ---------
cep | CEP que você deseja obter as informações

## Códigos dos Serviços dos Correios

Código | Descrição
------ | ---------
04014 | SEDEX à vista
04065 | SEDEX à vista pagamento na entregra
04510 | PAC à vista
04707 | PAC à vista pagamento na entrega
40169 | SEDEX 12 (à vista e a faturar)
40215 | SEDEX 10 (à vista e a faturar)
40290 | SEDEX Hoje Varejo

## Preços e Prazos de Caixas

```shell
curl "http://api.kcep.run/85801-001/01505-010/16x11x2/1"
  -H "x-api-key: DyYOx4YvF5uCrJGWkxPy7wDYNcansaq8VTo4unX8"
```

> O comando acima irá retornar uma estrutura JSON semelhante a esta:

```json
{
  "data": [
    {
      "Codigo": 4014,
      "Valor": "48,40",
      "PrazoEntrega": "3",
      "ValorMaoPropria": "0,00",
      "ValorAvisoRecebimento": "0,00",
      "ValorValorDeclarado": "0,00",
      "EntregaDomiciliar": "S",
      "EntregaSabado": "S",
      "Erro": "0",
      "MsgErro": "",
      "ValorSemAdicionais": "48,40",
      "obsFim": "",
      "DescricaoCodigo": "SEDEX à vista"
    },
    {
      "Codigo": 4065,
      "Valor": "63,87",
      ...
    },
    {
      "Codigo": 4510,
      "Valor": "23,20",
      ...
    },
    {
      "Codigo": 4707,
      "Valor": "38,67",
      ...
    },
    {
      "Codigo": 40169,
      "Valor": "0",
      ...
    },
    {
      "Codigo": 40215,
      "Valor": "0",
      ...
    },
    {
      "Codigo": 40290,
      "Valor": "0,00",
      ...
    }
  ],
  "updatedAt": "2018-04-01T19:46:55.966Z"
}
```

Através deste endpoint você obtem os dados de preço e prazo de diferentes serviços dos correios. Por padrão retornamos
as informações para todos os serviços disponíveis, mas também oferecemos alguns parâmetros para filtrar as informações.

### Requisição HTTP

`GET https://api.kcep.run/<cepOrigem>/<cepDestino>/<comprimento>x<largura>x<altura>/<peso>`

### Parâmetros de Consulta

Parâmetro | Obrigatório | Padrão | Descrição
--------- | ----------- | ------ | ---------
cepOrigem | Sim | Nenhum | CEP de origem da encomenda
cepDestino | Sim | Nenhum | CEP de destino da encomenda
comprimento | Sim | 0 | Comprimento da embalagem (deve respeitar a [tabela](#dimensoes-para-caixas) abaixo)
largura | Sim | 0 | Largura da embalagem (deve respeitar a [tabela](#dimensoes-para-caixas) abaixo)
altura | Sim | 0 | Altura da embalagem (deve respeitar a [tabela](#dimensoes-para-caixas) abaixo)
peso | Sim | 0 | Peso da encomenda com a embalagem (entre 0 e 30kg). Ex.: 1.575, 1.2, 2, etc
servico | Não | Todos | [Serviços dos correios](#codigos-dos-servicos-dos-correios) separados por vírgula. Ex.: 04014,04510
maoPropria | Não | N | Indica se a encomenda será entregue com o serviço adicional mão própria. Informe S ou N.
valorDeclarado | Não | 0 | Encomenda será entregue com o serviço adicional valor declarado quando diferente de 0.
avisoRecebimento | Não | N | Indica se a encomenda será entregue com o serviço adicional aviso de recebimento. Informe S ou N.

<aside class="notice">
Os parâmetros opcionais devem ser informados como parâmetro de URL, ex.: <code>?servico=04014,04510&avisoRecebimento=S</code>
</aside>

### Dimensões para Caixas

Dimensão | Mínimo | Máximo
-------- | ------ | ------
Comprimento (C) | 16cm | 105cm
Largura (L) | 11cm | 105cm
Altura (A) | 2cm | 105cm
Soma (C+L+A) | 29cm | 200cm

## Preços e Prazos de Envelopes

```shell
curl "http://api.kcep.run/85801-001/01505-010/envelope/16x11/1"
  -H "x-api-key: DyYOx4YvF5uCrJGWkxPy7wDYNcansaq8VTo4unX8"
```

> O comando acima irá retornar uma estrutura JSON semelhante a esta:

```json
{
  "data": [
    {
      "Codigo": 4014,
      "Valor": "48,40",
      "PrazoEntrega": "3",
      "ValorMaoPropria": "0,00",
      "ValorAvisoRecebimento": "0,00",
      "ValorValorDeclarado": "0,00",
      "EntregaDomiciliar": "S",
      "EntregaSabado": "S",
      "Erro": "0",
      "MsgErro": "",
      "ValorSemAdicionais": "48,40",
      "obsFim": "",
      "DescricaoCodigo": "SEDEX à vista"
    },
    ...
  ],
  "updatedAt": "2018-04-01T19:46:55.966Z"
}
```

Através deste endpoint você obtem os dados de preço e prazo de diferentes serviços dos correios. Por padrão retornamos
as informações para todos os serviços disponíveis, mas também oferecemos alguns parâmetros para filtrar as informações.

### Requisição HTTP

`GET https://api.kcep.run/<cepOrigem>/<cepDestino>/<comprimento>x<largura>/<peso>`

### Parâmetros de Consulta

Parâmetro | Obrigatório | Padrão | Descrição
--------- | ----------- | ------ | ---------
cepOrigem | Sim | Nenhum | CEP de origem da encomenda
cepDestino | Sim | Nenhum | CEP de destino da encomenda
comprimento | Sim | 0 | Comprimento da embalagem (deve respeitar a [tabela](#dimensoes-para-envelopes) abaixo)
largura | Sim | 0 | Largura da embalagem (deve respeitar a [tabela](#dimensoes-para-envelopes) abaixo)
peso | Sim | 0 | Peso da encomenda com a embalagem (entre 0 e 1kg)
servico | Não | Todos | [Serviços dos correios](#codigos-dos-servicos-dos-correios) separados por vírgula. Ex.: 04014,04510
maoPropria | Não | N | Indica se a encomenda será entregue com o serviço adicional mão própria. Informe S ou N
valorDeclarado | Não | 0 | Encomenda será entregue com o serviço adicional valor declarado quando diferente de 0
avisoRecebimento | Não | N | Indica se a encomenda será entregue com o serviço adicional aviso de recebimento. Informe S ou N

<aside class="notice">
Os parâmetros opcionais devem ser informados como parâmetro de URL, ex.: <code>?servico=04014,04510&avisoRecebimento=S</code>
</aside>

### Dimensões para Envelopes

Dimensão | Mínimo | Máximo
-------- | ------ | ------
Comprimento (C) | 16cm | 60cm
Largura (L) | 11cm | 60cm
Soma (C+L) | 27cm | 120cm

## Preços e Prazos de Rolos

```shell
curl "http://api.kcep.run/85801-001/01505-010/rolo/18x5/1"
  -H "x-api-key: DyYOx4YvF5uCrJGWkxPy7wDYNcansaq8VTo4unX8"
```

> O comando acima irá retornar uma estrutura JSON semelhante a esta:

```json
{
  "data": [
    {
      "Codigo": 4014,
      "Valor": "68,40",
      "PrazoEntrega": "3",
      "ValorMaoPropria": "0,00",
      "ValorAvisoRecebimento": "0,00",
      "ValorValorDeclarado": "0,00",
      "EntregaDomiciliar": "S",
      "EntregaSabado": "S",
      "Erro": "0",
      "MsgErro": "",
      "ValorSemAdicionais": "48,40",
      "obsFim": "",
      "DescricaoCodigo": "SEDEX à vista"
    },
    ...
  ],
  "updatedAt": "2018-04-01T20:36:50.910Z"
}
```

Através deste endpoint você obtem os dados de preço e prazo de diferentes serviços dos correios. Por padrão retornamos
as informações para todos os serviços disponíveis, mas também oferecemos alguns parâmetros para filtrar as informações.

### Requisição HTTP

`GET https://api.kcep.run/<cepOrigem>/<cepDestino>/<comprimento>x<diametro>/<peso>`

### Parâmetros de Consulta

Parâmetro | Obrigatório | Padrão | Descrição
--------- | ----------- | ------ | ---------
cepOrigem | Sim | Nenhum | CEP de origem da encomenda
cepDestino | Sim | Nenhum | CEP de destino da encomenda
comprimento | Sim | 0 | Comprimento da embalagem (deve respeitar a [tabela](#dimensoes-para-rolos) abaixo)
diametro | Sim | 0 | Diâmetro da embalagem (deve respeitar a [tabela](#dimensoes-para-rolos) abaixo)
peso | Sim | 0 | Peso da encomenda com a embalagem (entre 0 e 30kg). Ex.: 1.575, 1.2, 2, etc
servico | Não | Todos | [Serviços dos correios](#codigos-dos-servicos-dos-correios) separados por vírgula. Ex.: 04014,04510
maoPropria | Não | N | Indica se a encomenda será entregue com o serviço adicional mão própria. Informe S ou N
valorDeclarado | Não | 0 | Encomenda será entregue com o serviço adicional valor declarado quando diferente de 0
avisoRecebimento | Não | N | Indica se a encomenda será entregue com o serviço adicional aviso de recebimento. Informe S ou N

<aside class="notice">
Os parâmetros opcionais devem ser informados como parâmetro de URL, ex.: <code>?servico=04014,04510&avisoRecebimento=S</code>
</aside>

### Dimensões para Rolos

Dimensão | Mínimo | Máximo
-------- | ------ | ------
Comprimento (C) | 18cm | 105cm
Diâmetro (D) | 5cm | 91cm
Soma (C+2D) | 28cm | 200cm