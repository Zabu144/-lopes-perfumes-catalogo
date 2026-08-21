# Lopes Perfumes — Site do Catálogo

Site simples, sem servidor, sem banco de dados e sem senha de admin.
Um único arquivo (`index.html`) contém tudo: visual, catálogo e o botão de
finalizar pedido no WhatsApp.

## Como funciona

- O cliente navega pelo catálogo, adiciona perfumes ao carrinho (ou clica
  direto em "💬" para perguntar sobre um item específico).
- Ao finalizar, o site monta uma mensagem já formatada com nome, itens,
  total e endereço, e abre o WhatsApp automaticamente com tudo preenchido.
- Não existe backend: nada fica "salvo" no site. O pedido em si só existe
  a partir do momento em que chega no WhatsApp — exatamente como já
  funciona hoje na prática.

## Como editar o catálogo (adicionar/tirar perfume, mudar preço)

1. Abra o arquivo `index.html` em qualquer editor de texto (até o Bloco de
   Notas funciona, mas o [VS Code](https://code.visualstudio.com/) é mais
   confortável).
2. Procure o trecho `const PRODUTOS = [` (use Ctrl+F).
3. Cada perfume é um bloco assim:

```js
{ id: 1, nome: "Eau de Parfum Paris", marca: "Importado", preco: 150.00,
  descricao: "Perfume sofisticado e elegante...", estoque: 10, emoji: "🌹" },
```

- **Adicionar um perfume**: copie um bloco inteiro, cole no final da lista
  (antes do `];`), mude o `id` para um número que ainda não existe, e
  ajuste os outros campos.
- **Remover um perfume**: apague o bloco inteiro dele.
- **Mudar preço/estoque**: só trocar o número.
- `estoque: 0` marca o perfume como "Esgotado" automaticamente.
  `estoque` entre 1 e 5 mostra "Últimas unidades".

Salve o arquivo depois de editar.

## Sobre as fotos dos perfumes

As fotos usadas agora são placeholders de banco de imagens gratuito (Pixabay,
licença livre para uso comercial) — servem só para o site não ficar com
espaço vazio enquanto vocês não têm fotos reais dos produtos.

Para trocar por uma foto real:

1. Tire ou consiga a foto do perfume (funciona até foto de celular, se tiver
   boa luz e fundo neutro).
2. Hospede essa imagem em algum lugar acessível por link — pode ser no
   próprio GitHub (subindo a foto pro repositório, numa pasta tipo `imagens/`),
   ou em qualquer serviço de hospedagem de imagem.
3. No `index.html`, ache o produto correspondente dentro de `PRODUTOS` e troque
   o valor de `imagem` pelo novo link (ou pelo caminho do arquivo, se você subiu
   a foto para dentro do próprio repositório, tipo `imagem: "imagens/paris.jpg"`).

## Como trocar o número de WhatsApp

Perto do início do `<script>`, tem:

```js
const WHATSAPP_NUMERO = "5582998380780";
const WHATSAPP_EXIBICAO = "(82) 99838-0780";
```

Troque pelos dados corretos (número sempre com código do país + DDD, só
números, sem espaço nem traço no `WHATSAPP_NUMERO`).

## Como publicar o site (gratuito)

A forma mais simples, sem precisar mexer em terminal ou linha de comando:

### Opção A — Netlify Drop (mais fácil possível)
1. Acesse **https://app.netlify.com/drop**
2. Arraste o arquivo `index.html` para a página
3. Pronto — o site já fica no ar com um link tipo `nome-aleatorio.netlify.app`
4. É possível depois configurar um domínio próprio (ex: `lopesperfumes.com.br`)
   direto no painel do Netlify, se vocês comprarem um domínio (Registro.br
   para `.com.br`, por exemplo)

### Opção B — Vercel (se preferirem, também é gratuito)
1. Crie uma conta em vercel.com
2. "Add New Project" → "Deploy" → arraste a pasta com o `index.html`
3. Mesmo processo de domínio próprio depois, se quiserem

Nos dois casos: **sempre que quiser atualizar o catálogo**, é só editar o
`index.html` de novo e repetir o mesmo processo de arrastar o arquivo.

## Próximos passos possíveis (não obrigatórios)

Isso aqui é a base mais simples possível. Se o negócio crescer e fizer
sentido, dá para evoluir depois, por exemplo:

- Trocar a lista `PRODUTOS` fixa por uma **planilha do Google Sheets
  publicada como CSV**, para editar o catálogo sem precisar mexer em
  código — só editando a planilha.
- Adicionar fotos reais dos perfumes (hoje usa emojis como ícone).
- Comprar um domínio próprio (`.com.br`) para ficar mais profissional.

Nenhum desses passos é necessário para o site funcionar — são só
possibilidades para quando fizer sentido.
