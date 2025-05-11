# fnGeradorBoletos

Azure Function para geração de códigos de barras bancários, com imagem base64 e envio de mensagem para o Azure Service Bus.

---

## Visão Geral

Essa Azure Function (`barcode-generate`) recebe via HTTP POST dois parâmetros:

- `valor` (ex: `"49.90"`)
- `dataVencimento` (formato: `"yyyy-MM-dd"`)

Com esses dados, ela:
- Valida e formata os dados recebidos.
- Gera um código de barras bancário com 44 caracteres.
- Cria uma imagem do código de barras (formato Code128, base64).
- Retorna os dados via HTTP.
- Envia a mesma estrutura para uma fila do Azure Service Bus.

---

## 🚀 Como funciona?

- [Validação dos Dados]
- [Conversão de valor e data]
- [Geração de Código de Barras (44 caracteres)]
- [Geração da imagem (Code128)]
- [Envio para Azure Service Bus]
- [Retorno JSON com código e imagem]
- [Retorno JSON para o cliente]



## Exemplo de Requisição

**Endpoint:**
```
POST /api/barcode-generate
```

**Body (JSON):**
```json
{
  "valor": "49.90",
  "dataVencimento": "2025-06-30"
}
```

---

## Exemplo de Resposta

```json
{
  "barcode": "00820250630499000...",
  "valorOriginal": 49.90,
  "DataVencimento": "2025-05-16T10:44:22.123Z",
  "ImagemBase64": "iVBORw0KGgoAAAANSUhEUgAA..."
}
```

> Obs: `DataVencimento` no retorno é sempre a data atual + 5 dias

---

## Configuração

Antes de executar, configure a variável de ambiente:

```bash
ServiceBusConnectionString=<sua-connection-string-do-azure-service-bus>
```

A fila usada é:

```bash
gerador-codigo-barras
```

---

## Dependências

- `Azure.Messaging.ServiceBus`
- `BarcodeStandard`
- `SkiaSharp`
- `Newtonsoft.Json`
- `Microsoft.Azure.Functions.Worker`

---

## 📌 Possibilidades de Expansão

| Funcionalidade | Descrição |
|----------------|-----------|
| Segurança | Adicionar autenticação JWT ou API Key |
| Armazenamento | Salvar imagens em Azure Blob Storage |
| Monitoramento | Integrar com Application Insights |
