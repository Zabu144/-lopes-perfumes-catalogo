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

## Como gerenciar o catálogo (adicionar/tirar perfume, mudar preço)

O catálogo agora vem de uma **planilha do Google Sheets**. Depois de
configurada uma vez, adicionar ou remover perfume é só editar a planilha —
não precisa mais mexer no código nem subir nada no GitHub de novo.

### Configuração inicial (fazer uma única vez)

1. Baixe o arquivo `catalogo-modelo.csv` que eu gerei — ele já vem com as
   colunas certas e os 6 perfumes de exemplo preenchidos.
2. Acesse [sheets.google.com](https://sheets.google.com) e crie uma
   planilha em branco.
3. No menu, vá em **Arquivo → Importar → Fazer upload**, selecione o
   `catalogo-modelo.csv` e escolha a opção **"Substituir planilha"**.
   Isso já deixa a planilha pronta com as colunas certas.
4. Agora vá em **Arquivo → Compartilhar → Publicar na Web**.
   - Em "Link", selecione a aba da planilha (geralmente "Página1")
   - Em formato, escolha **Valores separados por vírgula (.csv)**
   - Clique em **Publicar** e confirme
5. Copie o link gerado (algo como
   `https://docs.google.com/spreadsheets/d/e/.../pub?output=csv`).
6. Abra o `index.html`, procure por `GOOGLE_SHEETS_CSV_URL = ""` e cole o
   link entre as aspas:
   ```js
   const GOOGLE_SHEETS_CSV_URL = "https://docs.google.com/spreadsheets/d/e/SEU-LINK-AQUI/pub?output=csv";
   ```
7. Suba esse `index.html` atualizado no GitHub (essa é a **única vez** que
   você vai precisar mexer em código para isso).

### No dia a dia (seu pai pode fazer isso sozinho)

Com tudo configurado, basta abrir a planilha no navegador ou no app do
Google Sheets e:

- **Adicionar um perfume**: criar uma nova linha, preenchendo todas as
  colunas. O `id` precisa ser um número que ainda não foi usado.
- **Remover um perfume**: apagar a linha inteira (clique com o botão
  direito no número da linha → "Excluir linha").
- **Mudar preço ou estoque**: só editar o número na célula.
- **Marcar como esgotado**: colocar `0` na coluna `estoque`.

As mudanças aparecem no site automaticamente na próxima vez que alguém
visitar (não precisa publicar de novo nem subir nada no GitHub).

### Colunas da planilha

| Coluna | O que é | Exemplo |
|---|---|---|
| `id` | Número único do perfume (não repetir) | `7` |
| `nome` | Nome do perfume | `Essência Real` |
| `marca` | Marca ou "Importado" | `Importado` |
| `preco` | Preço, use ponto para decimais | `159.90` |
| `descricao` | Frase curta descrevendo o perfume | `Aroma amadeirado e intenso` |
| `estoque` | Quantidade disponível (0 = esgotado) | `5` |
| `imagens` | Uma ou mais fotos do perfume (veja abaixo) | `link1\|link2\|link3` |

Se por algum motivo a planilha não carregar (sem internet, link errado),
o site automaticamente volta a mostrar o catálogo de exemplo, então ele
nunca fica completamente vazio ou quebrado.

### Como adicionar fotos (inclusive várias por perfume)

Na coluna `imagens`, você pode colocar **até 3 fotos por perfume**,
separadas pelo símbolo `|` (barra vertical), sem espaço antes ou depois:

```
https://link-da-foto-1.jpg|https://link-da-foto-2.jpg|https://link-da-foto-3.jpg
```

Se for só uma foto, coloca só o link normalmente, sem o `|`. No site, a
primeira foto aparece no card do catálogo; ao clicar no perfume, todas as
fotos aparecem numa galeria que dá pra passar (setas ou bolinhas).

**Usando fotos do Google Drive**: é só usar o link de compartilhamento
normal do Drive (aquele que você pega em "Compartilhar" → "Copiar link").
O site já converte esse link automaticamente para o formato de imagem —
não precisa fazer nada especial. Só uma observação importante: o arquivo
no Drive precisa estar com o compartilhamento configurado como
**"Qualquer pessoa com o link"** (não "Restrito"), senão a imagem não
carrega para quem visita o site, porque o Google exige essa permissão
para servir a imagem publicamente.

Se algum link de imagem quebrar ou não carregar por qualquer motivo, o
site mostra um ícone simples de frasco no lugar — nunca fica com o
"quadrado quebrado" de imagem ausente.

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

## Prévia bonita ao compartilhar o link (Open Graph)

Quando alguém manda o link do site no WhatsApp, Instagram, etc., o site já
está preparado para mostrar uma prévia com a logo, título e descrição, em
vez de aparecer só o link cru.

**Falta um passo pra isso funcionar**: essa prévia precisa de uma imagem
hospedada com endereço público (diferente do favicon, ela não pode ficar
"embutida" no código). Eu já gerei essa imagem (`og-image.png`).

1. Suba o arquivo `og-image.png` para o **mesmo repositório do GitHub**,
   na raiz (mesmo lugar do `index.html`).
2. Depois que o site estiver publicado e você souber o endereço final dele
   (ex: `lopes-perfumes-catalogo.vercel.app`), abra o `index.html` e troque
   todas as ocorrências de `https://SEU-SITE-AQUI.vercel.app` pelo endereço
   real do site (tem 3 ocorrências, procure por `SEU-SITE-AQUI`).
3. Suba essa versão atualizada — pronto, a prévia passa a funcionar.

Se pular esse passo, o site continua funcionando normalmente — só o link
aparece sem prévia ao ser compartilhado.

## Favicon (ícone da aba do navegador)

Isso já está pronto e funcionando — o ícone da logo já aparece na aba do
navegador automaticamente, sem precisar de nenhuma configuração adicional.

## Perguntas frequentes (FAQ)

Adicionei uma seção de perguntas frequentes entre o catálogo e o rodapé,
no formato de acordeão (clica na pergunta e a resposta expande). Isso ajuda
a responder de antemão as dúvidas mais comuns antes do cliente chegar no
WhatsApp: formas de pagamento, entrega, se os perfumes são originais,
política de troca, e prazo de entrega.

**Importante**: escrevi respostas de exemplo/placeholder para essas
perguntas — revise com calma e ajuste o texto pra bater exatamente com
como o negócio do seu pai funciona de verdade (por exemplo, se ele aceita
cartão além de Pix e dinheiro, se entrega fora da cidade, etc.).

Para editar, abra o `index.html` e procure por `<section class="faq">`.
Cada pergunta e resposta está num bloco assim:

```html
<div class="faq-item">
  <button class="faq-pergunta">
    Quais formas de pagamento vocês aceitam?
    <span class="faq-icone">+</span>
  </button>
  <div class="faq-resposta">
    <div class="faq-resposta-inner">
      Aceitamos Pix e dinheiro. Combine a forma de pagamento diretamente
      pelo WhatsApp ao finalizar o pedido.
    </div>
  </div>
</div>
```

Para **adicionar uma pergunta nova**, copie um bloco inteiro desses e cole
antes do fechamento de `</div>` da lista, ajustando a pergunta e a resposta.
Para **remover**, apague o bloco inteiro.

## Próximos passos possíveis (não obrigatórios)

Se o negócio crescer e fizer sentido, dá para evoluir depois, por exemplo:

- Trocar as fotos placeholder por fotos reais dos perfumes.
- Comprar um domínio próprio (`.com.br`) para ficar mais profissional.
- Configurar as automações nativas do WhatsApp Business (saudação
  automática, mensagem de ausência, respostas rápidas) — feito direto no
  aplicativo do WhatsApp Business, sem precisar de código.
- Manter um histórico de pedidos numa planilha separada (registro
  automático) — não implementado por enquanto, mas pode ser adicionado
  depois se fizer sentido.

Nenhum desses passos é necessário para o site funcionar — são só
possibilidades para quando fizer sentido.
