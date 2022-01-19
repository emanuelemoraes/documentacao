## Obtendo token de autenticação

Para obter o token necessário para realizar as chamadas na API de integração do módulo de impressoras
é preciso realizar uma chamada na API do NDD Identity com os seguintes dados:

``` json	
	method: POST
	url: https://auth.nddorbix.com/connect/token	
	body:
		grant_type: client_credentials
		scope: {scope}
		client_id: {client_id}
		client_secret: {secret}
```		

>Consulte o suporte da NDD para obter o client_id/secret

### Escopos disponíveis
  
|API| Scope |
|--|--|
| Impressoras | nddsmart-printer-external-integration-api |
| Estruturação do Provedor | nddsmart-core-external-integration-api |

>Para obter o token mais de um escopo pode ser utilizado em uma mesma chamada
		
## Parâmetros obrigatórios das chamadas

Para conseguir realizar chamadas na API de integração do módulo de impressoras 
é necessário informar o nome do provedor e o token de autenticação:

	header:
		authorization: Bearer {token}
		tenant: {nome do provedor}
		

## Filtros de busca

A API de integração do Módulo de impressoras tem suporte a filtros OData, confira abaixo os parâmetros disponíveis:

``` json	
	url: https://{{PrinterExternalApi}}/v1/printer-reference-counter?$select=referenceCounterName&$orderby=referenceCounterName
	query:
		$count=true
		&$skip=01
		&$top=5
		&$select=id
		&$orderby=name
		&$filter=name eq A4 Mono
```

### Parâmetros

`$count` - Habilita o retorno da quantidade de registros encontrados

`$skip` - Número de registros a serem pulados

`$top` - Número máximo de registros a serem retornados

`$select` - Seleciona quais propriedades retornar

`$orderby` - Determina qual propriedade ordenará os registros

`$filter` - Uma função que deve ser verdadeiro para o registro ser retornado

>Documentação - https://docs.microsoft.com/pt-br/azure/search/search-query-odata-filter

## Paginação

A API de integração do Módulo de impressoras tem suporte a paginação server-side, confira abaixo os parâmetros disponíveis:
	
		url: https://{{PrinterExternalApi}}/v1/printer-reference-counter?$count=true&$pagesize=50
		query:
			$count=true
			$pagesize=50

### Parâmetros

`$count` - Para funcionar a paginação é necessário habilitar o **count** que retorna a quantidade de registros encontrados

`$pagesize` - Número de registros por página - `Valor padrão: 500`, `Limite máximo: 500`

### Resposta
  
```json
{
	"items": [
		{
			"id": 6,
			"name": "Antares"
		},
		{
			"id": 7,
			"name": "John"
		},
		{
			"id": 8,
			"name": "Doe"
		}
		...
	],
	"nextPageLink": "https://localhost:44325/employees?$skip=50",
	"count": 100
}
```
### Parâmetros

`count` - Quantidade de registros encontrados

`nextPageLink` - Contém um link para a próxima página se houver mais registros disponíveis

# Api Externa de Integração do Módulo de Impressoras

O objetivo da API de integração é possibilitar ao usuário automatizar seu processo de fechamento de acordo com as regras de contadores de referência definidas no ORBIX, também visa a possibilidade de inserção de contadores na base do Orbix a fim de utilizar tais dados aliados com as features do sistema para reposição de suprimentos ou aplicação das regras de referência.
